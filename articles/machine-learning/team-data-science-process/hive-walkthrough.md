---
title: Bir Hadoop kümesinde - Team Data Science Process verilerini keşfedin
description: Team Data Science Process, yapı ve model dağıtma için bir HDInsight Hadoop kümesi kullanan bir uçtan uca senaryo için kullanma.
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
ms.openlocfilehash: 005d4fe1b6ec59e7f05be3dd2ab3e72d0e7aa8e0
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78393371"
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Team Data Science Process'in çalışması: kullanımı Azure HDInsight Hadoop kümeleri
Bu kılavuzda, [ekip veri bilimi işlemini (TDSP)](overview.md) uçtan uca bir senaryoda kullanırız. Genel kullanıma açık [NYC Taxi](https://www.andresmh.com/nyctaxitrips/) veri kümesinden verileri depolamak, araştırmak ve özellik mühendislerini ve verileri aşağı [örneklemek için bir Azure HDInsight Hadoop kümesi](https://azure.microsoft.com/services/hdinsight/) kullanırız. İkili ve çok sınıflı sınıflandırma ve regresyon Tahmine dayalı görevler işlemek üzere Azure Machine Learning ile veri modelleri ekleriz. 

Daha büyük bir veri kümesini nasıl işleyeceğinizi gösteren bir anlatım için, bkz. [Team Data Science Process-1-TB veri kümesinde Azure HDInsight Hadoop kümeleri kullanma](hive-criteo-walkthrough.md).

Ayrıca, 1 TB 'lık veri kümesini kullanan yönergede sunulan görevleri gerçekleştirmek için bir IPython Not defteri de kullanabilirsiniz. Daha fazla bilgi için bkz. [HIVE ODBC bağlantısı kullanan Criteo anlatımı](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb).

## <a name="dataset"></a>NYC TAXI gidiş veri kümesi açıklaması
Yaklaşık 20 GB sıkıştırılmış virgülle ayrılmış değerler (CSV) dosyaları (~ 48 sıkıştırılmamış GB) NYC taksi seyahat verilerdir. 173 milyondan fazla bireysel gelişlerin sahiptir ve her seyahat için ücretli fares içerir. Her bir seyahat kaydı toplama ve dropoff konumu ve zaman, anonim hack (sürücü) lisans numarası ve medallion sayı (benzersiz Tanımlayıcı taksi ait) içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

- Trip_data CSV dosyaları seyahat ayrıntıları içeriyor: pascuların sayısı, seçme ve açılan noktaları, seyahat süresi ve seyahat uzunluğu. Birkaç örnek kayıt şunlardır:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
- Trip_fare CSV dosyaları her seyahat için ödenen tarifeli havayolu ayrıntılarını içerir: ödeme türü, tarifeli havayolu miktarı, ek ücret, vergiler, ipuçları ve Tolls ve ödenen toplam tutar. Birkaç örnek kayıt şunlardır:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat\_veri ve seyahat\_tarifeli havayolu katılacak benzersiz anahtar alanlardan oluşur: medtalon, Hack\_lisansı ve\_tarih saat toplama. Tüm Ayrıntılar için belirli bir seyahat ilgili almak için bu üç anahtar ile katılmak yeterli olur.

## <a name="mltasks"></a>Tahmin görevlerinin örnekleri
Gerekli işlem görevlerinin açıklanmasına yardımcı olmak için veri analizinden göre yapmak istediğiniz tahmine yönelik türü belirleme. Bu kılavuzda, *ipucu\_miktarına*bağlı olarak, bu kılavuzda yer alan tahmin sorunlarına yönelik üç örnek verilmiştir:

- **İkili sınıflandırma**: bir tıp bir seyahat için ödenip ödenmediğini tahmin edin. Diğer bir deyişle, $0 'den büyük bir *\_miktarı* pozitif bir örnektir, ancak *İpucu\_$0 miktarı* negatif bir örnektir.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
- **Birden çok Lass sınıflandırması**: seyahat için ödenen ipucu tutarlarının aralığını tahmin edin. *İpucu\_miktarını* beş sınıfa bölyoruz:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
- **Regresyon görevi**: seyahat için ödenen ipucunun miktarını tahmin edin.  

## <a name="setup"></a>Gelişmiş analiz için bir HDInsight Hadoop kümesi ayarlama
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Bir HDInsight kümesi üç adımda kullanan gelişmiş analiz için bir Azure ortamı ayarlayabilirsiniz:

1. [Depolama hesabı oluştur](../../storage/common/storage-account-create.md): Bu depolama hesabı, verileri Azure Blob depolama alanında depolamak için kullanılır. HDInsight kümelerinde kullanılan verileri de burada yer alıyor.
2. [Gelişmiş analiz işlemi ve teknoloji için Azure HDInsight Hadoop kümelerini özelleştirin](customize-hadoop-cluster.md). Bu adım, 64-bit Anaconda Python 2.7 tüm düğümlerde yüklü olan bir HDInsight Hadoop kümesi oluşturur. HDInsight kümenizi özelleştirirken unutmayın gereken iki önemli adımlar vardır.
   
   * Oluşturduğunuz zaman, HDInsight kümenizle 1. adımda oluşturduğunuz depolama hesabına bağlanmak unutmayın. Küme içinde işlenen verileri bu depolama hesabına erişir.
   * Kümeyi oluşturduktan sonra küme baş düğümüne uzaktan erişimi etkinleştirin. **Yapılandırma** sekmesine gidin ve **Uzaktan Etkinleştir**' i seçin. Bu adım, uzaktan oturum açma için kullanılan kullanıcı kimlik bilgilerini belirtir.
3. [Azure Machine Learning çalışma alanı oluştur](../studio/create-workspace.md): Machine Learning modelleri oluşturmak için bu çalışma alanını kullanın. Bu görev ilk veri İnceleme tamamladıktan sonra aşağı örnekleme, HDInsight kümesi kullanarak değinilmiştir.

## <a name="getdata"></a>Verileri ortak bir kaynaktan al
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

[NYC TAXI gidiş](https://www.andresmh.com/nyctaxitrips/) veri kümesini makinenize ortak konumundan kopyalamak Için [verileri Azure Blob depolama alanına taşıma](move-azure-blob.md)bölümünde açıklanan yöntemlerden herhangi birini kullanın.

Burada, AzCopy verilerini içeren dosyaları aktarmak için nasıl kullanılacağını açıklar. AzCopy indirmek ve yüklemek için [AzCopy komut satırı yardımcı programı ile çalışmaya](../../storage/common/storage-use-azcopy.md)başlama bölümündeki yönergeleri izleyin.

1. Komut istemi penceresinde, aşağıdaki AzCopy komutlarını çalıştırın, *\<path_to_data_folder >* istenen hedefle değiştirin:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Kopyalama tamamlandıktan sonra seçilen veri klasöründeki 24 sıkıştırılmış dosyaların toplam görürsünüz. Yerel makinenizde indirilen dosyaları aynı dizine ayıklayın. Sıkıştırılması kaldırılan dosyaların bulunduğu klasör not edin. Bu klasör, aşağıdaki *\_\_unzipped_data\_dosyalar\>\<yol* olarak adlandırılır.

## <a name="upload"></a>HDInsight Hadoop kümesinin varsayılan kapsayıcısına verileri yükleme
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Aşağıdaki AzCopy komutları, Hadoop kümesi oluştururken belirttiğiniz gerçek değerlerle aşağıdaki parametreleri değiştirin ve veri dosyalarını sıkıştırması açılıyor.

* ***\<path_to_data_folder >*** Makinenizde, ZIP üzerinde olmayan veri dosyalarını içeren dizin (yol ile birlikte).  
* ***Hadoop kümesinin\<depolama hesabı adı >*** HDInsight kümeniz ile ilişkili depolama hesabı.
* ***Hadoop kümesinin varsayılan kapsayıcısını\<>*** Kümeniz tarafından kullanılan varsayılan kapsayıcı. Varsayılan kapsayıcının adı genellikle kümenin kendisiyle aynı addır. Örneğin, "abc123.azurehdinsight.net" Küme çağrılırsa, varsayılan kapsayıcı abc123 ' dir.
* ***\<depolama hesabı anahtarı >*** Kümeniz tarafından kullanılan depolama hesabı için anahtar.

Bir komut istemi veya bir Windows PowerShell penceresi, aşağıdaki iki AzCopy komutları çalıştırın.

Bu komut, seyahat verilerini Hadoop kümesinin varsayılan kapsayıcısındaki ***nyctaxitripraw*** dizinine yükler.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Bu komut, tarifeli havayolu verilerini Hadoop kümesinin varsayılan kapsayıcısındaki ***nyctaxifareraw*** dizinine yükler.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Verilerin şimdi Blob Depolama ve HDInsight küme içinde kullanılacak hazır olmanız gerekir.

## <a name="#download-hql-files"></a>Hadoop kümesinin baş düğümünde oturum açın ve araştırmacı veri analizi hazırlayın
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Keşif veri analizi ve verilerin aşağı örneklemesini sağlamak üzere kümenin baş düğümüne erişmek için, [Hadoop kümesinin baş düğümüne erişme](customize-hadoop-cluster.md)bölümünde özetlenen yordamı izleyin.

Bu kılavuzda, aslında SQL benzeri bir sorgu dili olan [Hive](https://hive.apache.org/)içinde yazılmış sorguları, ön veri araştırmalarını gerçekleştirecek şekilde kullanacağız. Hive sorguları '. HQL ' dosyalarında depolanır. Biz ardından aşağı bu veri modelleri oluşturmak için Machine Learning'i kullanılacak örnek.

Kümeyi araştırmacı veri analizi için hazırlamak üzere [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) 'Dan ilgili Hive betiklerini içeren '. HQL ' dosyalarını baş düğümde bir yerel dizine (C:\Temp) indirin. Kümenin baş düğümü içinden komut istemi ' ni açın ve aşağıdaki iki komutu çalıştırın:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Bu iki komut, Bu izlenecek yolda gereken tüm '. HQL ' dosyalarını baş düğümündeki ***C:\Temp&#92;***  yerel dizinine indirir.

## <a name="#hive-db-tables"></a>Aya göre bölümlenmiş Hive veritabanı ve tabloları oluşturma
> [!NOTE]
> Bu görev genellikle yönetici içindir.
> 
> 

Hive tablolarını NYC taksi veri kümesi oluşturmak artık hazırsınız.
Hadoop kümesi baş düğümünde masaüstünde baş düğümü, Hadoop komut satırı açın. Aşağıdaki komutu çalıştırarak Hive dizin girin:

    cd %hive_home%\bin

> [!NOTE]
> Tüm Hive komutlarını bu izlenecek yolda Hive dönüşüm kutusundan Çalıştır / directory istemi. Bu yol sorunları otomatik olarak işler. "Hive directory istemi" terimleri kullanırız "Hive bin / directory istemi" ve bu kılavuzda birbirinin yerine "Hadoop komut satırı".
> 
> 

Hive Dizin isteminde, Hive veritabanını ve tablolarını oluşturan baş düğümün Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Bu, **Nyctaxidb**Hive veritabanını ve **seyahat** ve **tarifeli havayolu**tablolarını oluşturan **\_DB\_ve\_Tables. hql dosyası oluşturmak\_c:\temp\sample\_Hive** içeriğini aşağıda bulabilirsiniz.

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

## <a name="#load-data"></a>Bölümlere göre Hive tablolarına veri yükleme
> [!NOTE]
> Bu görev genellikle yönetici içindir.
> 
> 

Daha hızlı işleme ve sorgu süreleri etkinleştirmek için kullandığımız aya göre doğal bir bölümleme NYC taksi veri kümesi vardır. (Hive dizinden Hadoop komut satırını kullanarak verilen) aşağıdaki PowerShell komutlarını seyahat veri yük ve masrafları Hive tablolarını, aya göre bölümlere.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

**Örnek\_Hive,\_partitions. HQL dosyası tarafından\_\_\_veri yükleme** , aşağıdaki **yükleme** komutlarını içerir:

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Araştırma sürecinde burada kullanılan Hive sorgularının sayısı yalnızca bir veya iki bölüme bakmayı içerir. Ancak, veri kümesi genelinde bu sorguları çalıştırabilirsiniz.

### <a name="#show-db"></a>HDInsight Hadoop kümesindeki veritabanlarını gösterme
HDInsight Hadoop kümesinde Hadoop komut satırı penceresini içinde oluşturulan veritabanlarını göstermek için Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show databases;"

### <a name="#show-tables"></a>**Nyctaxidb** veritabanında Hive tablolarını gösterme
**Nyctaxidb** veritabanındaki tabloları göstermek Için, Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show tables in nyctaxidb;"

Aşağıdaki komutu çalıştırarak tabloları bölümlenir olduğunu doğrulayabilirsiniz:

    hive -e "show partitions nyctaxidb.trip;"

Beklenen çıktı aşağıdaki gibidir:

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

Benzer şekilde, aşağıdaki komutu çalıştırarak da taksi tablonun bölümlenme sağlayabilirsiniz:

    hive -e "show partitions nyctaxidb.fare;"

Beklenen çıktı aşağıdaki gibidir:

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

## <a name="#explore-hive"></a>Hive 'de veri araştırması ve özellik Mühendisliği
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Veri keşfi ve özellik mühendislik Hive tablolarına yüklenen veriler için görevleri gerçekleştirmek için Hive sorgularını kullanabilirsiniz. Bu tür görev örnekleri şunlardır:

* Her iki tabloda ilk 10 kayıtları görüntüleyin.
* Değişen zaman pencereleri bazı alanların veri dağıtımları keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* İkili ve çok sınıflı sınıflandırma etiketleri ipucu miktarı üzerinden oluşturur.
* Özellikler, doğrudan seyahat uzaklıkları bilgi işlem tarafından oluşturur.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>İnceleme: ilk 10 kayıtları tablosu seyahat içinde görüntüleyin.
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Verilerin nasıl göründüğünü görmek için her tablodan 10 kayıt inceleyin. Kayıtları incelemek için aşağıdaki iki sorguları ayrı ayrı Hadoop komut satırı konsolu Hive directory isteminden çalıştırın.

İlk 10 kayıtları, ilk ay içinde dönüş tablo almak için:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

İlk 10 kayıtları, ilk ay taksi tabloda almak için:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Önceki sorguda küçük bir değişiklik ile uygun bir şekilde görüntülemek için kayıtları bir dosyaya kaydedebilirsiniz:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>İnceleme: kayıt sayısını 12 bölümlerin her görüntüleyin
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Dönüş sayısı takvim yılı nasıl değişeceğini ilgi çekecektir. Gruplandırma ölçütü: month gelişlerin dağılımını gösterir.

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

Burada ilk sütun ayı ve saniyedir söz konusu ay için dönüş sayısı.

Biz de toplam kayıt sayısı, Hive directory isteminde aşağıdaki komutu çalıştırarak seyahat veri kümemizdeki güvenebilirsiniz:

    hive -e "select count(*) from nyctaxidb.trip;"

Bu komut şunları verir:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Seyahat veri kümesi için gösterilen benzer komutları kullanarak Hive sorguları kayıt sayısını doğrulamak taksi veri kümesi için Hive directory isteminden verebilir.

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

Hive directory isteminden aşağıdaki komutu kullanarak kayıt taksi kümesindeki toplam sayısını güvenebilirsiniz:

    hive -e "select count(*) from nyctaxidb.fare;"

Bu komut şunları verir:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Her iki tablodaki kayıtların toplam sayısı aynı zamanda, verilerin doğru şekilde yüklendiğinden ikinci bir doğrulama sağlamak için de aynıdır.

### <a name="exploration-trip-distribution-by-medallion"></a>İnceleme: Seyahat dağıtım medallion tarafından
> [!NOTE]
> Bu analiz genellikle bir veri bilimi görevidir.
> 
> 

Bu örnekte belirli bir süre içinde (taksi numaraları) medallions 100 gelişlerin daha büyük tanımlar. Bölümlenmiş tablo erişiminizden sorgu avantajları, Bölüm değişkeninin **aya**göre belirlenir. Sorgu sonuçları, baş düğümdeki `C:\temp` bir yerel dosyaya, **quergı put. tsv**dosyasına yazılır.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

**Örnek\_hive\_seyahat\_sayı\_, inceleme için\_medtalon. HQL** dosyasında bulunan içeriği aşağıda verilmiştir.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medallion NYC taksi kümesindeki benzersiz bir cab tanımlar. Hangi kabinler hangilerinin birden çok belirli bir dönüş sayısı belirli bir süre içinde yapılan isteyerek daha meşgul olduğunu belirleyebilirsiniz. Aşağıdaki örnek, ilk üç ayda yüz 'dan fazla bir dönüşten daha fazla olan Cabs 'yi tanımlar ve sorgu sonuçlarını **C:\temp\quer47put.exe**yerel dosyasına kaydeder.

**Örnek\_hive\_seyahat\_sayı\_, inceleme için\_medtalon. HQL** dosyasında bulunan içeriği aşağıda verilmiştir.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Hive directory isteminden aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>İnceleme: Seyahat dağıtım medallion ve hack lisans tarafından
> [!NOTE]
> Bu görev genellikle bir veri bilimcisini için kullanılır.
> 
> 

Bir veri kümesini araştırırken, genellikle değer gruplarının dağıtımlarını incelemek istiyoruz. Bu bölüm, cab ve sürücüler için bu çözümlemenin nasıl yapılacağını gösteren bir örnek sağlar.

**Örnek\_hive\_\_count\_\_,\_lisansında hack_license. HQL** dosyası, tarifeli havayolu veri kümesini **medalon** veüzerinde gruplandırır ve her bir birleşimin sayısını döndürür. İçeriği şunlardır:

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Bu sorgu, dönüş sayısı göre azalan düzende sıralı, cab ve sürücü birleşimlerini döndürür.

Hive directory isteminde çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Sorgu sonuçları, **C:\temp\quer, put.exe**adlı yerel bir dosyaya yazılır.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitude-or-latitude-records"></a>Keşfetme: geçersiz bir boylam veya enlem kayıtlar için işaretleyerek veri kalitesi değerlendiriliyor
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Geçersiz veya bozuk kayıtları süzer ortadan kaldırmak için keşif veri analizi, ortak bir hedefi olan. Bu bölümdeki örnek, enlem veya boylam alanları kadar NYC alanı dışında bir değer içeren olup olmadığını belirler. Tür kayıtları hatalı boylam enlem değeri olması olası olduğundan, model için kullanılacak olan veri bunları ortadan kaldırmak istiyoruz.

İnceleme için **Assessment. HQL dosyasının\_quality\_örnek\_Hive** içeriği aşağıda verilmiştir.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Hive directory isteminde çalıştırın:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Bu komutta yer alan *-S* bağımsız değişkeni Hive harita/azaltma işlerinin durum ekranı çıktısını bastırır. Bu komut, Hive sorgu çıkışının ekran yazdırmasını daha okunaklı hale getiren yararlı olur.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>İnceleme: İkili sınıfı dağıtımlarını seyahat ipuçları
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

[Tahmin görevleri örnekleri](hive-walkthrough.md#mltasks) bölümünde özetlenen ikili sınıflandırma sorunu için, bir ipucunun verilip verilmediğini bilmemiz yararlı olur. Bu dağıtımı ipuçları, ikili:

* İpucu verildi (sınıf 1, ipucu\_tutar > $0)  
* İpucu yok (Sınıf 0, ipucu\_tutarı = $0)

Aşağıdaki **örnek\_hive\_eğimli\_frekansları. HQL** dosyası çalıştırılacak komutu gösterir:

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Hive directory isteminde çalıştırın:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>İnceleme: dağıtımlar çok sınıflı ayarında sınıfı
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

[Tahmin görevleri örnekleri](hive-walkthrough.md#mltasks) bölümünde özetlenen birden çok Lass sınıflandırma sorunu için, bu veri kümesi aynı zamanda verilen ipuçlarının miktarını tahmin etmek için kendisini doğal bir sınıflandırmayla da özetler. Sorgu ipucu aralıklarını tanımlamak için depo kullanabiliriz. Çeşitli tıp aralıklarına yönelik sınıf dağıtımlarını almak için, **örnek\_hive\_tıp\_aralık\_frekansları. HQL** dosyasını kullanın. İçeriği aşağıda verilmiştir.

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

### <a name="exploration-compute-the-direct-distance-between-two-longitude-latitude-locations"></a>İnceleme: boylam enlem iki konum arasında doğrudan uzaklık işlem
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Doğrudan arasındaki uzaklığı iki konum, taksi gerçek seyahat mesafesini arasında bir fark olup olmadığını bilmek isteyebilirsiniz. Bir yolcu bunlar sürücü kasıtlı olarak bunlar daha uzun bir yol tarafından gerçekleştirilen olduğunu anlamasına ipucu olma olasılığını olabilir.

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

Yukarıdaki sorguda, R mil olarak dünya yüzdesi olan ve PI radyana dönüştürülür. Boylam-Enlem noktaları, NYC alanından uzak değerleri kaldıracak şekilde filtrelenmiştir.

Bu durumda, sonuçları **querbir putdir**adlı dizine yazdık. Aşağıdaki komutları dizisini ilk olarak bu çıkış dizinini oluşturur ve ardından Hive komut çalıştırır.

Hive directory isteminde çalıştırın:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Sorgu sonuçları, Hadoop kümesinin varsayılan kapsayıcısı altında dokuz Azure Blob 'a (**quer0,0,putdir/000000 yazın\_0** - **quer0,0,putdir/000008\_0**) yazılır.

Tek tek bloblar boyutunu görmek için Hive directory komut isteminden aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///queryoutputdir

Belirli bir dosyanın içeriğini görmek için, **000000 yazın\_0**deyin, Hadoop 'un `copyToLocal` komutunu kullanın.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal` büyük dosyalar için çok yavaş olabilir ve bunlarla birlikte kullanılması önerilmez.  
> 
> 

Bu verilerin bir Azure blobuna sahip olmasının önemli bir avantajı, [verileri Içeri aktarma][import-data] modülünü kullanarak Machine Learning içindeki verileri keşfedebiliriz.

## <a name="#downsample"></a>Machine Learning 'de aşağı örnek veri ve derleme modelleri
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Keşif verileri analiz aşamadan sonra artık Machine Learning modeli oluşturmak için verilerin aşağı-sample hazırız. Bu bölümde, bir Hive sorgusu aşağı örnek verileri nasıl kullanacağınızı göstereceğiz. Machine Learning [verileri Içeri aktarma][import-data] modülünden erişir.

### <a name="down-sampling-the-data"></a>Aşağı-örnekleme verileri
Bu yordamda iki adımı vardır. İlk olarak, **nyctaxidb. seyahat** ve **nyctaxidb. tarifeli havayolu** tablolarını tüm kayıtlarda bulunan üç anahtara katıyoruz: **medtalon**, **Hack\_lisansı**ve **\_tarih saat**. Daha sonra bir ikili sınıflandırma etiketi, **eğimli**ve çok sınıflı bir sınıflandırma etiketi, **tıp\_sınıfı**oluşturacağız.

Machine Learning ' deki [verileri Içeri aktarma][import-data] modülünden doğrudan örnek olarak kullanabilmek için, önceki sorgunun sonuçlarını bir iç Hive tablosuna depolamanız gerekir. Aşağıda, biz iç bir Hive tablosu oluşturmak ve içeriğini birleştirilmiş ve alt örneklenen verileri ile doldurun.

Sorgu, **toplama\_tarih saat** alanından aşağıdaki zaman parametrelerini oluşturmak için doğrudan standart Hive işlevlerini uygular:
- günün saati
- yılın haftası
- hafta içi (' 1 ' Pazartesi için gelir ve ' 7 ' Pazar günü temsil eder)

Sorgu, toplama ve dropoff konumlar arasında doğrudan uzaklık de oluşturur. Bu işlevlerin tüm listesi için bkz. [Languagemanual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF).

Sorgu ardından aşağı-veri böylece sorgu sonuçlarını Azure Machine Learning Studio'ya sığabilen örnekleri. Özgün veri kümesinden yalnızca yaklaşık yüzde 1 Studio'ya içeri aktarılır.

Aşağıda,\_model oluşturmaya yönelik verileri hazırlayan **\_AML Machine Learning Full. HQL dosyası için hazırlama\_\_örnek\_Hive** içerikleri verilmiştir:

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

Bu sorgu Hive directory isteminden çalıştırmak için:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Artık, Machine Learning [veri alma][import-data] modülü kullanılarak erişilebilen **nyctaxidb. nyctaxi_downsampled_dataset**iç tablosuna sahipsiniz. Ayrıca, ki bu veri kümesi makine öğrenimi modelleri oluşturmak için kullanabilirsiniz.  

### <a name="use-the-import-data-module-in-machine-learning-to-access-the-down-sampled-data"></a>Verileri içeri aktarma modülü Machine Learning'de alt örneklenen verilere erişmek için kullanın.
Machine Learning [veri Içeri aktarma][import-data] modülünde Hive sorguları vermek için, bir Machine Learning çalışma alanına erişmeniz gerekir. Ayrıca, küme ve ilişkili depolama hesabı kimlik bilgilerini erişim gerekir.

[Veri Içeri aktarma][import-data] modülü ve giriş için parametreler hakkında bazı ayrıntılar aşağıda verilmiştir:

**Hcatalog sunucusu URI 'si**: küme adı **abc123**ise, şunu kullanın: https://abc123.azurehdinsight.net.

**Hadoop Kullanıcı hesabı adı**: küme için seçilen Kullanıcı adı (Uzaktan Erişim Kullanıcı adı değil).

**Hadoop Kullanıcı hesabı parolası**: küme için seçilen parola (uzaktan erişim parolası değil).

**Çıkış verilerinin konumu**: Azure olarak seçildi.

**Azure depolama hesabı adı**: kümeyle ilişkili varsayılan depolama hesabının adı.

**Azure Container Name**: küme için varsayılan kapsayıcı adı ve genellikle küme adıyla aynıdır. **Abc123**adlı bir küme için ad abc123 olur.

> [!IMPORTANT]
> Machine Learning [veri alma][import-data] modülünü kullanarak sorgulamak istediğimiz herhangi bir tablo, iç tablo olmalıdır.
> 
> 

Burada, **D. db** veritabanındaki **bir tablonun iç tablo olup** olmadığı nasıl saptanamıyor. Hive directory isteminden aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///D.db/T

İç tablo tablodur ve doldurulmuş, içeriğini buraya göstermeniz gerekir.

Bir tablo iç tablo olup olmadığını belirlemek için başka bir yolu, Azure Depolama Gezgini kullanmaktır. Kümenin varsayılan kapsayıcı adını gidin ve ardından tablo adına göre filtrelemek için kullanın. Tablo ve içeriği gösterilirse, bu iç tablo olduğunu doğrular.

Hive sorgusunun ve [veri Içeri aktarma][import-data] modülünün bir ekran görüntüsü aşağıda verilmiştir:

![Verileri içeri aktarma modülü için ekran görüntüsü, Hive sorgusu](./media/hive-walkthrough/1eTYf52.png)

Aşağı örneklerimizin verileri varsayılan kapsayıcıda bulunduğundan, Machine Learning elde edilen Hive sorgusu basittir. Yalnızca bir **select * from nyctaxidb. nyctaxi\_downörneklenmiş\_verileri**.

Veri kümesi, makine öğrenimi modelleri oluşturmak için başlangıç noktası olarak artık kullanılabilir.

### <a name="mlmodel"></a>Machine Learning modelleri derleme
Artık [Machine Learning](https://studio.azureml.net)' de model oluşturma ve model dağıtımına devam edebilirsiniz. Verileri, bizim daha önce tanımlanan tahmin sorunları gidermede kullanmak için hazırdır:

- **İkili sınıflandırma**: bir ipucunun seyahat için ödenip ödenmediğini tahmin etmek için.

  **Öğrenner:** İki sınıf Lojistik gerileme

  a. Bu sorun için, hedef (veya sınıf) etiketi **eğimli**olur. Özgün alt örneklenen veri kümesi bu sınıflandırma deneme için hedef sızıntılarını olan birkaç sütun içeriyor. Özellikle, **ipucu\_sınıfı**, **tıp\_tutarı**ve **Toplam\_tutarı** , test sırasında kullanılamayan hedef etiketle ilgili bilgileri açığa çıkarır. Bu sütunları, [veri kümesindeki sütunları seçme][select-columns] modülünde kullanarak göz önünde çıkardık.

  Bizim deneme ipucu için belirli bir seyahat Ücretli olup olmadığını tahmin etmek için aşağıdaki diyagramda gösterilmiştir:

  ![İpucu ödenmiş tahmin etmek için deneme diyagramı](./media/hive-walkthrough/QGxRz5A.png)

  b. Bu deneme için bizim hedef etiket dağıtımların yaklaşık 1:1 yoktu.

   Aşağıdaki grafikte, ikili sınıflandırma sorunu için sınıf etiketleri ipucu dağılımını gösterir:

  ![Dağıtım ipucu sınıfı etiketlerin grafik](./media/hive-walkthrough/9mM4jlD.png)

    Sonuç olarak, biz 0.987, eğrisini (AUC) altında bir alan aşağıdaki şekilde gösterildiği gibi alın:

  ![Grafik AUC değeri](./media/hive-walkthrough/8JDT0F8.png)

- **Birden çok Lass sınıflandırması**: daha önce tanımlanan sınıfları kullanarak seyahat için ödenen ipucu tutarlarının aralığını tahmin etmek için.

  **Öğrenner:** Birden çok Lass Lojistik gerileme

  a. Bu sorun için, hedef (veya sınıf) etiketimiz, beş değerden birini (0, 1, 2, 3, 4) kapsayan **ipucu\_sınıfıdır**. İkili sınıflandırma durumda olduğu gibi bu deneme için hedef sızıntılarını olan birkaç sütunlar sahibiz. Belirli, **eğimli**, **İpucu\_tutarı**ve **Toplam\_miktarı** , test sırasında kullanılamayan hedef etiketle ilgili bilgileri açığa çıkarır. Bu sütunları, [veri kümesindeki sütunları seç][select-columns] modülünde kullanarak kaldırdık.

  Aşağıdaki diyagramda, hangi Kutusu'na bir ipucu kalan olasılığı tahmin etmek için deneme gösterilmektedir. Depo olan: sınıf 0: ipucu 0 ABD Doları, sınıf 1 =: ipucu > $0 ve ipucu < 5 ABD Doları, sınıf 2 =: ipucu > $5 ve ipucu < 10 ABD Doları, sınıf 3 =: ipucu > $10 ve ipucu < = 20 ve sınıf 4: > $20 ipucu.

  ![İpucu için depo tahmin etmek için deneme diyagramı](./media/hive-walkthrough/5ztv0n0.png)

  Şimdi gerçek test sınıfı dağıtımını nasıl göründüğünü gösterir. Sınıf 0 ve sınıf 1 sık karşılaşılan ve diğer sınıflar işlemleri nadiren gerçekleşir.

  ![Test sınıfı dağılım grafiği](./media/hive-walkthrough/Vy1FUKa.png)

  b. Bu deneme için, burada gösterildiği gibi tahmin accuracies bakmak için bir karışıklık matrisi kullanırız:

  ![Karışıklık Matrisi](./media/hive-walkthrough/cxFmErM.png)

  Yaygın sınıfların accuracies sınıfı iyi olsa da, model, rarer sınıflarında "öğrenme" işleminin iyi bir işini yapmaz.

- **Regresyon görevi**: seyahat için ödenen ipucu miktarını tahmin etmek için.

  **Öğrenner:** Artırılmış karar ağacı

  a. Bu sorun için, hedef (veya sınıf) etiketi **tip\_tutardır**. Bu durumda hedef sızıntıları şunlardır: **eğimli**, **tıp\_sınıfı**ve **Toplam\_miktarı**. Tüm bu değişkenler, test süresi genellikle kullanılabilir olan ipucu miktarı hakkında bilgi gösterir. Bu sütunları, [veri kümesindeki sütunları seç][select-columns] modülünde kullanarak kaldırdık.

  Verilen bahşiş miktarını tahmin etmek için deneme Aşağıdaki diyagramda gösterilmiştir:

  ![İpucu miktarı tahmin etmek için deneme diyagramı](./media/hive-walkthrough/11TZWgV.png)

  b. Regresyon karşılaştığınız sorunları, tahminler ve katsayısı karesi alınmış hata bakarak biz öngörme doğruluk ölçü:

  ![Tahmin istatistikleri ekran görüntüsü](./media/hive-walkthrough/Jat9mrz.png)

  Burada, katsayısı 0.709 olduğu gibi yaklaşık yüzde 71 farkı olduğunu belirtmek modeli katsayıları tarafından açıklanmıştır.

> [!IMPORTANT]
> Machine Learning hakkında daha fazla bilgi edinmek ve nasıl erişebileceğiniz ve kullanılacağı hakkında daha fazla bilgi için bkz. [Machine Learning](../studio/what-is-machine-learning.md). Ayrıca, [Azure yapay zeka Galerisi](https://gallery.cortanaintelligence.com/) denemeleri bir gamutu kapsamakta ve Machine Learning özellikleri hakkında kapsamlı bir giriş sağlar.
> 
> 

## <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve eşlik eden kodlarını MIT lisansı altında Microsoft tarafından paylaşılır. Daha fazla bilgi için GitHub 'daki örnek kodun dizinindeki **LICENSE. txt** dosyasına bakın.

## <a name="references"></a>Başvurular
• [Andr, Monroy NYC Taxi seyahatler Indirme sayfası](https://www.andresmh.com/nyctaxitrips/)  
• [Cwhong  NYC 'Nin TAXI veri yolculuğu](https://chriswhong.com/open-data/foil_nyc_taxi/)  
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



