---
title: 1 TB veri kümesi üzerinde Azure HDInsight Hadoop kümesi kullanma-takım veri bilimi Işlemi
description: Bir HDInsight Hadoop kümesi oluşturun ve büyük bir (1 TB) genel kullanıma açık veri kümesini kullanarak bir model dağıtma kullanan bir uçtan uca senaryo için Team Data Science Process kullanma
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
ms.openlocfilehash: 218fb96f6960e194f0fc4a4a3a3e603388b961c8
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76760819"
---
# <a name="the-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>Ekip veri bilimi Işlemi, 1 TB 'lik bir veri kümesinde Azure HDInsight Hadoop kümesi kullanılarak yapılır.

Bu izlenecek yol bir uçtan uca senaryo ile Team Data Science Process kullanma gösterir bir [Azure HDInsight Hadoop kümesi](https://azure.microsoft.com/services/hdinsight/) depolamak için keşfetmek, özellik mühendisi ve aşağı herkese birindenörnekveriler[ Criteo](https://labs.criteo.com/downloads/download-terabyte-click-logs/) veri kümeleri. Azure Machine Learning, bu veriler üzerinde bir ikili sınıflandırma modeli oluşturmak için kullanır. Ayrıca bu modellerden biri, bir Web hizmeti olarak yayımlama işlemini de gösterir.

Bu izlenecek yolda gösterilen görevler gerçekleştirmek için Ipython notebook kullanmak da mümkündür. Bu yaklaşım denemek ister misiniz kullanıcılar [Criteo izlenecek bir Hive ODBC bağlantısı kullanarak](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) konu.

## <a name="dataset"></a>Criteo veri kümesi açıklaması
Criteo verileri, 4.300.000.000 ' den fazla kayıttan oluşan 370 GB gzip sıkıştırılmış TSV dosyası (~ 1,3 TB sıkıştırılmamış) olan bir tıklama tahmini veri kümesidir. 24 gün alınmış tarafından kullanıma sunulan veri tıklayın [Criteo](https://labs.criteo.com/downloads/download-terabyte-click-logs/). Veri bilimcileri kolaylık sağlamak için bize deneme amaçlı kullanılabilen veri sıkıştırması kaldırıldı.

Bu veri kümesi her kayıt, 40 sütunları içerir:

* İlk sütun bir kullanıcı tıkladığında olup olmadığını belirten bir etiket sütundur bir **ekleme** (değer 1) veya bir (0 değeri) tıklayın değil
* sonraki 13 sütunlar sayısal, ve
* Son 26 kategorik sütunlardır

Sütunları anonim hale getirilen ve bir dizi numaralandırılmış adlarını kullanın: için (için etiket sütunu) "Col1" ' Col40 "(için son kategorik sütun).

İlk 20 sütun bu veri kümesine ilişkin iki gözlemler (satır) bir alıntı şu şekildedir:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb

Bu veri kümesinde içinde hem bir sayısal ve kategorik sütunlar eksik değerleri bulunur. Eksik değerleri işlemek için basit bir yöntem açıklanır. Bunları Hive tablolarına depolarken verilerin ek ayrıntılar incelenmektedir.

**Tanım:** *tıklama oranı (Mrk):* Bu ölçüm, verilerdeki tıklama yüzdesinin yüzdesidir. Bu Criteo veri kümesinde, yaklaşık %3.3 veya 0.033 CTRL değil.

## <a name="mltasks"></a>Tahmin görev örnekleri
İki örnek tahmin sorunların bu kılavuzda ele alınmıştır:

1. **İkili sınıflandırma**: bir kullanıcı ekleme tıkladı olup olmadığını tahmin eder:

   * Sınıf 0: Yok tıklayın
   * Sınıf 1: tıklayın
2. **Regresyon**: bir ad tıklatma kullanıcı özelliklerinden olasılığını tahmin eder.

## <a name="setup"></a>Veri bilimi için ayarlanmış yukarı bir HDInsight Hadoop kümesi
> [!NOTE]
> Bu adım genellikle bir **Yönetim** görevidir.

Üç adımda HDInsight kümeleri ile Tahmine dayalı analiz çözümleri oluşturmak için Azure veri bilimi ortamınızı ayarlayın:

1. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md): Bu depolama hesabı, verileri Azure Blob Storage'da depolamak için kullanılır. HDInsight kümelerinde kullanılan veri burada depolanır.
2. [Veri bilimi için Azure HDInsight Hadoop kümelerini özelleştirin](customize-hadoop-cluster.md): Bu adım, 64-bit Anaconda Python 2.7 tüm düğümlerde yüklü olan Azure HDInsight Hadoop kümesi oluşturur. HDInsight küme özelleştirirken tamamlamak için (Bu konuda açıklanan) iki önemli adımlar vardır.

   * Adım 1 ' de oluşturulan depolama hesabını HDInsight kümenizin oluşturulduğu sırada bağlayın. Bu depolama hesabı, küme içinde işlenebilecek verilere erişmek için kullanılır.
   * Oluşturulduktan sonra kümenin baş düğümüne uzaktan erişimi etkinleştirin. Burada belirttiğiniz uzaktan erişim kimlik bilgilerini hatırlayın (küme oluşturulurken belirtilen kimlik bilgilerinden farklı): aşağıdaki yordamları uygulayın.
3. [Azure Machine Learning Studio (klasik) çalışma alanı oluştur](../studio/create-workspace.md): Bu Azure Machine Learning çalışma alanı, HDInsight kümesindeki bir ilk veri araştırması ve azaltma örneklemesi sonrasında makine öğrenimi modelleri oluşturmak için kullanılır.

## <a name="getdata"></a>Alma ve bir genel kaynaktan alınan verileri kullanma
[Criteo](https://labs.criteo.com/downloads/download-terabyte-click-logs/) bağlantısına tıklayarak, kullanım koşullarını kabul ederek ve bir ad sağlayarak, veri kümesi erişilebilir. Burada bir anlık görüntü gösterilir:

![Criteo koşullarını kabul edin](./media/hive-criteo-walkthrough/hLxfI2E.png)

Tıklayın **indirme devam et** daha fazla bilgi için veri kümesi ve kullanılabilirliğini hakkında.

Veriler bir [Azure Blob depolama](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) konumunda bulunuyor: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "wasb" Azure Blob Depolama konumunuz anlamına gelir.

1. Bu Azure Blob depolama alanındaki veriler, daraltılmış verilerin üç alt grubundan oluşur.

   1. Alt *ham/sayısı/* ilk 21 günlük verileri - gün içeren\_gününe kadar günleri 00\_20
   2. Alt *ham/eğitme/* verileri tek bir günü oluşur gün\_21
   3. Alt *ham/test/* verilerin iki gün oluşur gün\_22 ve gün\_23
2. Ham gzip verileri, *RAW/* as day_NN. gz ana klasöründe de mevcuttur, burada nn 00 ile 23 arasında gider.

Bir alternatif bir yaklaşım erişmek için keşfetmek ve Hive tablolarını oluşturduğumuzda yerel yüklemeleri gerektirmez bu veriler daha sonra bu kılavuzda açıklanan modeli.

## <a name="login"></a>Küme baş düğümüne oturum açın
Küme baş düğümüne bağlanmak için [Azure portalında](https://ms.portal.azure.com) küme bulunamıyor. HDInsight fil simgesine sol tıklayın ve sonra kümenizi adına çift tıklayın. **Yapılandırma** sekmesine gidin, sayfanın altındaki Bağlan simgesine çift tıklayın ve istendiğinde, bu bilgileri kümenin baş düğümüne ' a götüryerek uzaktan erişim kimlik bilgilerinizi girin.

Küme oluşturma düğümündeki tipik bir ilk oturum açma işlemi şöyle görünür:

![Küme oturum açın](./media/hive-criteo-walkthrough/Yys9Vvm.png)

Sol taraftaki "Hadoop komut bizim workhorse veri keşfi için olan" çizgidir. İki yararlı URL'leri - "Hadoop Yarn durumu" ve "Hadoop adı düğüm" dikkat edin. Yarn durumu URL işin ilerleme durumunu gösterir ve küme yapılandırmasına ayrıntılarını adı düğüm URL'sini verir.

Artık ayarlanır ve kılavuzun ilk bölümü başlamak için hazır: veri araştırması Hive kullanma ve Azure Machine Learning için verileri hazırlanıyor.

## <a name="hive-db-tables"></a> Hive veritabanı ve tablo oluşturma
Criteo kümemizi için Hive tabloları oluşturmak için açık ***Hadoop komut satırı*** baş düğümü, masaüstünde ve komutu girerek Hive dizini girin

    cd %hive_home%\bin

> [!NOTE]
> Tüm Hive komutlarını bu izlenecek yolda Hive dönüşüm kutusundan Çalıştır / directory istemi. Bu tüm yol sorunlarını otomatik olarak üstlenir. "Hive directory istemi" koşulları kullanabilirsiniz "Hive bin / directory istemi" ve "Hadoop komut satırı" birbirinin yerine.
>
> [!NOTE]
> Herhangi bir Hive sorgusu çalıştırmak için bir her zaman aşağıdaki komutları kullanabilirsiniz:
>
>        cd %hive_home%\bin
>        hive

Hive REPL ile göründükten sonra bir "hive >"oturum, yalnızca Kes ve onu yürütmek için sorguyu yapıştırın.

Aşağıdaki kod, "Criteo" veritabanını oluşturur ve ardından dört tablo oluşturur:

* bir *sayıları oluşturmak için tablo* gün günde oluşturulan\_gününe kadar günleri 00\_20,
* bir *eğitme veri kümesi olarak kullanılması tablo* günde oluşturulan\_21, ve
* iki *test veri kümesi olarak kullanılması için tabloları* günde oluşturulan\_22 ve gün\_23 sırasıyla.

Tatil günleri biri olduğu için test veri kümesini iki farklı tablolara böler. Amaç, model tıklama oranı gelen bir tatil ve tatil olmayan arasındaki farkları tespit edebilir belirlemektir.

Betik [örnek&#95;hive&#95;oluşturma&#95;criteo&#95;veritabanı&#95;ve&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) kolaylık olması için burada görüntülenir:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Tüm bu tablolar, Azure Blob depolama (ILB) konumlarına işaret edebilmeniz için dış bir yerdedir.

**HERHANGİ bir Hive sorgusu çalıştırmak için iki yolu vardır:**

* **HIVE REPL komut satırını kullanarak**: Birincisi bir "Hive" komutu vermektir ve Hive REPL komut satırına bir sorgu kopyalayıp yapıştırmaktır:

        cd %hive_home%\bin
        hive

     Şimdi REPL komut satırında, sorguyu kesip yapıştırarak yürütür.
* **Sorguları bir dosyaya kaydetme ve komutu yürütme**: İkincisi, sorguları bir '. HQL ' dosyasına ([örnek&#95;Hive&#95;oluşturma&#95;Criteo&#95;Database&#95;ve&#95;Tables. HQL](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) kaydetmek ve sonra sorguyu yürütmek için aşağıdaki komutu vermektir:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Veritabanı ve tablo oluşturma onaylayın
Ardından, aşağıdaki komutla Hive depo veritabanı oluşturulmasını onaylayın / directory istemi:

        hive -e "show databases;"

Bu sağlar:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Bu, "criteo" yeni bir veritabanı oluşturulmasını doğrular.

Hangi tabloları oluşturulan görmek için yalnızca Hive depo komutu Yürüt / directory istemi:

        hive -e "show tables in criteo;"

Ardından, aşağıdaki çıktıyı görmeniz gerekir:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="exploration"></a> Hive'da veri araştırması
Şimdi bazı temel veri keşfi kovanında adımlarına geçmeye hazırsınız. Tren örneklerde sayısı sayılarak başlatmak ve test veri tabloları.

### <a name="number-of-train-examples"></a>Train örnek sayısı
İçeriğini [örnek&#95;hive&#95;sayısı&#95;eğitme&#95;tablo&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) burada gösterilir:

        SELECT COUNT(*) FROM criteo.criteo_train;

Bu verir:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Alternatif olarak, bir de Hive depo aşağıdaki komutu verebileceği / directory istemi:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>İki test kümelerindeki test örnek sayısı
Şimdi iki test kümelerindeki örnek sayısı. İçeriğini [örnek&#95;hive&#95;sayısı&#95;criteo&#95;test&#95;gün&#95;22&#95;tablo&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) şunlardır:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Bu verir:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Her zamanki şekilde betik Hive dönüşüm kutusundan çağırıp / dizin komutu İstemi ile veren:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Gün bazında test veri kümesini test örneklerde sayısı son olarak, incelemeniz\_23.

Bunu yapmak için komut [gösterilenle benzerdir (örnek&#95;Hive&#95;Count&#95;Criteo&#95;test&#95;günü&#95;23&#95;örnek. HQL](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Bu sağlar:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Train kümesindeki etiket dağılımı
Etiket dağılımı train kümesindeki ilgi çekecektir. Bunu görmek için içeriğini göster [örnek&#95;hive&#95;criteo&#95;etiket&#95;dağıtım&#95;eğitme&#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Bu etiket dağılımı üretir:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Pozitif etiketlerin yüzdesi% 3,3 ' dir (özgün veri kümesiyle tutarlıdır).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogram dağıtımlarını train kümesindeki bazı sayısal değişkenler
Hive'nın yerel kullanabilirsiniz "histogram\_sayısal" sayısal değişkenler dağıtımını nasıl göründüğüne bulmak için işlevi. İşte içeriğini [örnek&#95;hive&#95;criteo&#95;histogram&#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Bu, aşağıdaki verir:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

Görünüm - YANAL normal listesi yerine bir SQL benzeri çıktı oluşturmak için Hive hizmet etmesi birlikte Aç. Bu tabloda, ilk sütun bin merkezine ve ikincisi de bin frekansına karşılık gelir.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Yaklaşık yüzdebirliklerini train kümesindeki bazı sayısal değişkenler
Ayrıca sayısal değişkenleriyle yaklaşık. yüzdebirlik değerleri hesaplama ilgilendirir. Hive yerel "yüzdebirlik\_yaklaşık" Bu bizim için yapar. İçeriğini [örnek&#95;hive&#95;criteo&#95;yaklaşık&#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) şunlardır:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Bu verir:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Yüzdebirliklerini dağıtımını yakından genellikle herhangi bir sayısal değişken histogram dağıtımını ilgilidir.

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Train kümesindeki bazı kategorik sütunlar için benzersiz değerlerin sayısını Bul
Veri keşfi devam ederek, bazı kategorik sütunlar için yapabilecekleri benzersiz değerlerin sayısını bulun. Bunu yapmak için içeriğini göster [örnek&#95;hive&#95;criteo&#95;benzersiz&#95;değerleri&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Bu verir:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Col15, 19D benzersiz değerlere sahip! "Sık erişimli bir kodlama" gibi naïve teknikleri kullanarak bu yüksek boyutlu kategorik değişkenleri kodlamak için uygun değildir. Özellikle, güçlü, sağlam bir teknik olarak adlandırılan [ile öğrenme sayar](https://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) verimli bir şekilde bu sorun giderme açıklanan kanıtlandı ve.

Son olarak bazı diğer kategorik sütunlar için de benzersiz değerlerin sayısını bakın. İçeriğini [örnek&#95;hive&#95;criteo&#95;benzersiz&#95;değerleri&#95;birden çok&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) şunlardır:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Bu verir:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Yeniden Col20 dışında diğer tüm sütunlar birçok benzersiz değerlere sahip olduğunu unutmayın.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Train kümesindeki kategorik değişkenlerin çiftleri ortak oluşum sayısı

Kategorik değişkenlerin çiftlerinin sayı dağıtımları da ilgi alanıdır. Bu kodu kullanarak belirlenebilir [örnek&#95;hive&#95;criteo&#95;eşleştirilmiş&#95;kategorik&#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Tersine, sayıları, oluşumunu göre sıralamak ve 15 üstünde bu durumda bakın. Bu bize sağlar:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a> Azure Machine Learning için aşağı örnek veri kümeleri
Veri kümeleri incelediniz ve Azure Machine Learning modelleri oluşturulabilir, bu tür bir araştırma (birleşimler dahil), tüm değişkenler için örnek veri kümelerini nasıl yapılacağı gösterilmiştir. Odak noktası sorun, geri çağırma: örnek öznitelikleri (Sütun2 - Col40 özellik değerleri) kümesi göz önünde bulundurulduğunda, Col1 0 (hiçbir tıklayın) veya 1 (tıklayın) olup olmadığını tahmin edin.

Örnek eğitme ve test veri kümeleri için %1 özgün boyutu, aşağı için Hive'nın yerel RAND() işlevini kullanın. Sonraki betik [örnek&#95;hive&#95;criteo&#95;alt örnekleyin&#95;eğitme&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) eğitme veri kümesi için bunu yapar:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Bu verir:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Betik [örnek&#95;hive&#95;criteo&#95;alt örnekleyin&#95;test&#95;gün&#95;22&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) test verileri için yaptığı gün\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Bu verir:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Son olarak, betik [örnek&#95;hive&#95;criteo&#95;alt örnekleyin&#95;test&#95;gün&#95;23&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) test verileri için yaptığı gün\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Bu verir:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Bu, bizim aşağı örneklenen eğitme ve Azure Machine learning'de oluşturmaya yönelik veri kümeleri test hazır olursunuz.

Azure Machine sayısı tablo ilgiliyse Learning için geçmeden önce son bir önemli bileşeni yoktur. Sonraki alt bölümde, Count tablosu bazı ayrıntılarla ele alınmıştır.

## <a name="count"></a> Kısa bir açıklama sayısı tablosunda
Gördüğünüz gibi, çeşitli kategorik değişkenlerin yüksek boyutlılık vardır. İzlenecek yolda, güçlü bir yöntem olarak adlandırılan [ile öğrenme sayar](https://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) Bu değişkenlere yapılan bir verimli kodlamak için sağlam bir şekilde sunulur. Bu yöntem hakkında daha fazla bilgi, sağlanan bağlantıdır.

>[!NOTE]
>Bu kılavuzda, yüksek boyutlu kategorik özellikleri compact temsillerini oluşturmak için sayısı tabloları kullanarak biridir. Bu kategorik özellikleri kodlamak için tek yolu değildir; ilgilenen kullanıcılar diğer teknikleri hakkında daha fazla bilgi için kullanıma [bir-sık erişimli-encoding](https://en.wikipedia.org/wiki/One-hot) ve [özellik karma](https://en.wikipedia.org/wiki/Feature_hashing).
>

Derleme sayısı veri sayısı tabloları için klasör ham/sayı verileri kullanın. Modelleme bölümde, bu, sıfırdan kategorik özellikleri sayısı tabloları oluşturmak nasıl kullanıcılarına gösterilir veya alternatif olarak kendi araştırmaları için önceden oluşturulmuş sayısı tablosu kullanılacak. Aşağıda içinde olduğunda "sayısı tabloları önceden oluşturulmuş" adlandırılır için sağlanmış olan sayısı tabloları kullanarak anlama. Sonraki bölümde bu tablolara erişen konusunda ayrıntılı yönergeler sağlanır.

## <a name="aml"></a> Azure Machine Learning ile model oluşturma
Oluşturma işlemi Azure Machine learning'de modelimiz, aşağıdaki adımları izler:

1. [Verileri Hive tabloları Azure Machine Learning içine alma](#step1)
2. [Deneme oluşturma: verileri temizleme ve sayı tablolarının bulunduğu bir özellik yapma](#step2)
3. [Oluşturma, eğitme ve modeli Puanlama](#step3)
4. [Modeli değerlendirme](#step4)
5. [Modeli web hizmeti olarak yayımlayın.](#step5)

Azure Machine Learning Studio'da modelleri oluşturmaya hazırsınız. Aşağı örneklenen verilerimizi, kümedeki Hive tablolarını olarak kaydedilir. Azure Machine Learning'i **verileri içeri aktarma** modülü bu verileri okumak için. Aşağıda bu küme, depolama hesabına erişmek için kimlik bilgilerini sağlanır.

### <a name="step1"></a> 1. adım: Azure Machine Learning kullanarak verileri içeri aktarma modül Hive tabloları veri almak ve bir makine öğrenimi denemesi için seçin
Başlangıç olarak bir **+ yeni** -> **deneme** -> **boş deneme**. Ardından **arama** sol, üst "Veri Al" için arama kutusu. Sürükle ve bırak **verileri içeri aktarma** modülü denemeyi açın tuval (ekranın orta kısmını) modülü veri erişimi için kullanılacak.

Bu nedir **verileri içeri aktarma** Hive tablosundaki verileri alınırken gibi görünür:

![Verileri içeri aktarma verileri alır](./media/hive-criteo-walkthrough/i3zRaoj.png)

İçin **verileri içeri aktarma** modülü, grafik olarak sağlanan parametrelerin değerlerini sağlamak için gereken değerleri tür yalnızca örnekleri verilmiştir. Bazı yönergeler aşağıda verilmiştir genel parametresi için ayarlanmış doldurmak nasıl **verileri içeri aktarma** modülü.

1. "Hive sorgusu" seçin **veri kaynağı**
2. İçinde **Hive veritabanı sorgusu** kutusuna, basit bir SELECT * FROM <,\_veritabanı\_name.your\_tablo\_adı >-yeterli olur.
3. **Hcatalog sunucusu URI**: "abc" kümeniz çalışıyorsa sonra sadece budur: https://abc.azurehdinsight.net
4. **Hadoop kullanıcı hesabı adı**: küme commissioning sırasında seçilen kullanıcı adı. (Uzaktan erişim kullanıcı adı değil!)
5. **Hadoop kullanıcı hesabı parolası**: küme commissioning sırasında seçilen kullanıcı adının parolası. (Uzaktan erişim parolayı değil!)
6. **Çıktı verilerini konumunu**: "Azure" seçin
7. **Azure depolama hesabı adı**: kümeyle ilişkili depolama hesabı
8. **Azure depolama hesabı anahtarı**: kümeyle ilişkili depolama hesabının anahtarı.
9. **Azure kapsayıcı adı**: "abc" Küme adıdır sonra yalnızca "abc", genellikle budur.

Bir kez **verileri içeri aktarma** tamamlandığında (gördüğünüz yeşil onay modülü üzerinde), veri alma (ile kendi tercih ettiğiniz bir ad) bir veri kümesi olarak bu verileri kaydedin. Ne bu şekilde görünür:

![Verileri içeri aktarma verileri kaydetme](./media/hive-criteo-walkthrough/oxM73Np.png)

Çıkış bağlantı noktasına sağ **verileri içeri aktarma** modülü. Bu gösteren bir **veri kümesi olarak Kaydet** seçeneği ve **Görselleştir** seçeneği. **Görselleştir** seçeneği seçeneğine tıkladıysanız, bazı Özet istatistikleri için kullanışlı olan bir sağ panelde yanı sıra veri 100 satırı görüntüler. Verileri kaydetmek için seçmeniz yeterlidir **veri kümesi olarak Kaydet** ve yönergeleri izleyin.

Veri kümeleri kullanılarak kaydedilmiş veri kümesini kullanmak için bir machine learning denemesine seçmek için bulun **arama** aşağıdaki şekilde gösterilen kutusu. Basit tür adı verdiğiniz veri kümesini kısmen erişim ve veri kümesi ana paneline sürükleyin. Ana panelinden bırakmadan kullanılmak üzere machine learning model seçilir.

![Veri kümesini ana panele sürükleyin](./media/hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> Bu, eğitin ve test veri kümeleri için yapın. Ayrıca, bu amaç için verdiğiniz tablo adları ve veritabanı adını kullandığınızdan emin olun. Şekilde, kullanılan değerler yalnızca gösterim yaratılır * için:
>
>

### <a name="step2"></a>2. Adım: tıklamaların tahmin edilmesi/tıklamaları tahmin etmek için Azure Machine Learning bir deneme oluşturma
Azure Machine Learning Studio (klasik) denemeniz şöyle görünür:

![Machine Learning denemesi](./media/hive-criteo-walkthrough/xRpVfrY.png)

Artık bu anahtar bileşenleri inceleyin. Kaydedilen eğdiğimiz tren ve test veri kümelerimizi önce deneme tuvalimize sürükleyin.

#### <a name="clean-missing-data"></a>Eksik Verileri Temizleme
**Eksik verileri temizleme** modülü mu ne adından da anlaşılacağı: kullanıcı tanımlı yollar eksik veriler temizler. Bunu görmek için bu modüle bakın:

![Eksik verileri temizleme](./media/hive-criteo-walkthrough/0ycXod6.png)

Burada, tüm eksik değerleri 0 ile değiştirmeyi seçin. Modül menülerde bakarak görülebilir diğer seçenekler de mevcuttur.

#### <a name="feature-engineering-on-the-data"></a>Veriler üzerinde özellik Mühendisliği
Büyük veri kümelerinin kategorik bazı özellikler için benzersiz değerler milyonlarca olabilir. Yüksek boyutlu gibi kategorik özellikleri temsil etmek için sık erişimli bir kodlama gibi naïve yöntemleri tamamen seçeneğinin kullanmaktır. Bu yönerge, bu yüksek boyutlu kategorik değişkenleri compact temsillerini oluşturmak için yerleşik Azure Machine Learning modüllerini kullanarak sayısı özelliklerinin nasıl kullanılacağını gösterir. Son sonuç, daha küçük bir model boyutudur, daha hızlı eğitim süreleridir ve diğer teknikleri kullanarak karşılaştırılabilir performans ölçümleridir.

##### <a name="building-counting-transforms"></a>Sayım oluşturma dönüşümleri
Sayısı özellikler oluşturmak için kullanın **derleme sayım dönüştürme** Azure Machine Learning'de kullanılabilen modülü. Modül şöyle görünür:

![Derleme dönüştürme sayma modülü özellikleri](./media/hive-criteo-walkthrough/e0eqKtZ.png)
![sayım dönüştürme derleme Modülü](./media/hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT]
> İçinde **sütunları sayma** kutusuna, sayıları üzerinde gerçekleştirmek istediğiniz sütunları girin. Genellikle, bu (belirtildiği gibi) yüksek boyutlu kategorik sütunlar. Criteo dataset 26 kategorik sütunlar içerdiğini unutmayın: Col15 Col40 için gelen. Burada, hepsinde saymak ve (gelen, 15 40 gösterildiği virgülle ayrılmış olarak) bunların dizinlerini verin.
>

Modül MapReduce modunda kullanmak için (büyük veri kümeleri için uygundur), bir HDInsight Hadoop kümesi erişmeniz (özellik araştırması için kullanılan bu amaç için yeniden kullanılabilir) ve kimlik bilgileri. Önceki rakamları aşağıdaki gibi görünür (kendi kullanım örneği için uygun değerlerle anlatımı için sağlanan değerler değiştirin) hangi doldurulmuş değerler gösterilmektedir.

![Modül parametrelerini](./media/hive-criteo-walkthrough/05IqySf.png)

Yukarıdaki şekilde, giriş blob konumuna girmek gösterilmektedir. Bu konum sayısı tablolar oluşturmak için ayrılmış verilere sahip.

Bu modül çalışmayı tamamladığında, dönüştürme için daha sonra modülü sağ tıklatıp seçerek Kaydet **dönüştürme Kaydet** seçeneği:

!["Dönüştürme Kaydet" seçeneği](./media/hive-criteo-walkthrough/IcVgvHR.png)

Yukarıda gösterilen deneme mimarimiz içinde tam olarak kaydedilen sayısı dönüşüm için veri kümesi "ytransform2" karşılık gelir. Bu deneyde geri kalanı için bu okuyucu kullandığınız varsayılır bir **derleme sayım dönüştürme** özellikleri eğitme ve test veri kümelerinde modülü bazı verileri sayıları oluşturup sonra bu sayıları sayısı oluşturmak için kullanabilirsiniz.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Hangi sayısı eğitme ve test kümelerinin dahil edilecek özellikleri seçme
Bir kez sayısı dönüştürme hazır kullanıcı kendi tren eklemek ve veri kümeleri kullanılarak test etmek için hangi özelliklerin seçebilir **değiştirme sayısı tablo parametreleri** modülü. Eksiksiz olması için bu modülü burada gösterilir. Ancak ilgi Basitlik, aslında bu bizim deneme kullanmayın.

![Sayısı tablo parametreleri değiştirin](./media/hive-criteo-walkthrough/PfCHkVg.png)

Bu durumda, görülebileceği gibi büyük olasılıkla günlük kullanılacak olan ve geri alma sütun göz ardı edilir. Çöp Kutusu eşik gibi parametreleri düzgünleştirme ve tüm Laplacian gürültü veya kullanıp kullanmayacağınızı eklemek için sözde önceki kaç örnekleri de ayarlayabilirsiniz. Tüm bu özellikler Gelişmiş ve bu varsayılan değerleri oluşturma özelliği bu tür için yeni olan kullanıcılar için iyi bir başlangıç noktası olan kaydedilmelidir.

##### <a name="data-transformation-before-generating-the-count-features"></a>Sayısı özellikleri oluşturmadan verileri dönüştürme
Odağı artık önemli bir bizim train dönüştürme hakkında gelin ve gerçekten sayısı özellikleri oluşturma önce veri test edin. Sayı dönüşümü verilerimize uygulanmadan önce iki **yürütme R betik** modülü kullanılır.

![R betik modüllerini yürütün](./media/hive-criteo-walkthrough/aF59wbc.png)

İlk R betiği şu şekildedir:

![İlk R betiği](./media/hive-criteo-walkthrough/3hkIoMx.png)

Bu R betiği adlarına "Col1" için "Col40" bizim sütunları yeniden adlandırır. Bu biçim adını sayısı dönüştürme bekliyor olmasıdır.

Pozitif ve negatif sınıflar arasında dağıtım ikinci R betiği dengeler (sırasıyla 1 ve 0 sınıflarını) aşağı-negatif sınıf örnekleme tarafından. R betiği buradan bunun nasıl yapılacağı gösterilmektedir:

![İkinci bir R betiği](./media/hive-criteo-walkthrough/91wvcwN.png)

Bu basit bir R betiği, "pos\_neg\_oranı" pozitif ve negatif sınıfları arasındaki dengeyi miktarı ayarlamak için kullanılır. Bu sınıf dağıtım olduğu sınıflandırma sorunları (Bu durumda, %3.3 pozitif ve negatif sınıfı %96.7 olduğunuz geri çağırma) dengesiz için iyileştirme sınıfı dengesizliği genellikle performans avantajlarının sahip olduğundan yapmak önemlidir.

##### <a name="applying-the-count-transformation-on-our-data"></a>Veri sayısı Dönüşüm Uygulama
Son olarak, kullanabileceğiniz **uygulamak dönüştürme** bizim trende sayısı Dönüşüm Uygulama ve veri kümeleri test etmek için modül. Bu modül, bir giriş olarak kaydedilen sayısı dönüştürme ve eğitme veya test veri kümeleri, diğer giriş olarak alır ve sayı özelliklere sahip verileri döndürür. Aşağıda gösterilmiştir:

![Dönüştürme modülü Uygula](./media/hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Bir alıntı özellikleri görünmesi sayısı
Sayısı özellikleri örneğimizde nasıl göründüğünü görmek için eğitici. Bu bir alıntı şu şekildedir:

![Sayısı özellikleri](./media/hive-criteo-walkthrough/FO1nNfw.png)

Bu alıntı sayılan sütunları, sayılarını Al ve tüm ilgili backoffs yanı sıra büyük olasılıkla oturum gösterir.

Dönüştürülen bu veri kümelerini kullanarak bir Azure Machine Learning modeli oluşturmak artık hazırsınız. Sonraki bölümde bu nasıl yapılabilir gösterilmektedir.

### <a name="step3"></a> 3. adım: Oluşturma, eğitme ve modeli Puanlama

#### <a name="choice-of-learner"></a>Learner seçimi
İlk olarak, bir learner seçmeniz gerekir. İki sınıflı artırmalı karar ağacı bizim learner kullanın. Bu learner için varsayılan seçenekleri şunlardır:

![İki sınıflı artırılmış karar ağacı parametreleri](./media/hive-criteo-walkthrough/bH3ST2z.png)

Deneme için varsayılan değerleri seçin. Varsayılanlar anlamlı ve performans üzerinde hızlı taban çizgileri almanın iyi bir yoludur. Temel oluşturduktan sonra isterseniz parametreleri üst düzey performansı artırabilir.

#### <a name="train-the-model"></a>Modeli eğitme
Eğitim için basitçe çağırmak bir **modeli eğitme** modülü. Bunu iki giriş iki sınıflı artırılmış karar ağacı learner ve eğitme kümemizi ' dir. Bu aşağıda gösterilmiştir:

![Train Model Modülü](./media/hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-the-model"></a>Modeli puanlama
Eğitilen bir modelin aldıktan sonra test veri kümesinde puanlamak için ve onun performansını değerlendirmek için hazır olursunuz. Kullanarak bunu **Score Model** modülü ile birlikte aşağıdaki şekilde gösterildiği bir **Evaluate Model** Modülü:

![Score Model (Model Puanlama) modülü](./media/hive-criteo-walkthrough/fydcv6u.png)

### <a name="step4"></a> 4. adım: modeli değerlendirme
Son olarak, model performansını çözümlemeniz gerekir. Genellikle, iki sınıf (ikili) sınıflandırma sorunu için iyi AUC ölçümüdür. Bu eğriyi görselleştirmek için, **puan modeli** modülünü bir **değerlendirme modeli** modülüne bağlayın. Tıklayarak **Görselleştir** üzerinde **Evaluate Model** modülü aşağıdakine benzer bir grafik verir:

![Modül BDT modeli değerlendirme](./media/hive-criteo-walkthrough/0Tl0cdg.png)

İkili dosya (veya iki sınıf) sınıflandırma sorunları tahmin doğruluğunu iyi bir ölçü olan alan altında eğri (AUC). Aşağıdaki bölümde, bu modeli test kümemizi kullanarak sonuçları gösterilmektedir. **Modeli değerlendir** modülünün çıkış bağlantı noktasına sağ tıklayın ve ardından **görselleştirin**.

![Evaluate Model modülü görselleştirin](./media/hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step5"></a> 5. adım: modeli bir Web hizmeti olarak yayımlayın.
Web hizmetleri fuss en az bir Azure Machine Learning modeli yayımlama olanağı, yaygın olarak kullanılabilir hale getirme için değerli bir özelliktir. Bu yapıldıktan sonra herkesin tahminler elde etmek için ihtiyaç duydukları ve web hizmeti, bu Öngörüler döndürülecek modeli kullanır. giriş verileriyle web hizmetine çağrı yapabilir.

İlk olarak eğitilen modelimizi eğitilen model nesnesi olarak kaydederek eğitim **modeli modülüne sağ** tıklayıp **eğitilen model olarak kaydet** seçeneğini kullanın.

Ardından, girdi oluşturma ve çıkış bağlantı noktasına web hizmetimiz için:

* bir giriş bağlantı noktasını tahminler elde etmek için gereken verileri ile aynı formda verileri alır.
* Puanlanmış etiketler ve ilişkili olasılıklar çıkış bağlantı noktasını döndürür.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Veri giriş bağlantı noktası için birkaç satırı seçin
Kullanmak uygun olan bir **geçerli SQL dönüştürme** giriş bağlantı noktasını verileri olarak görev yapacak yalnızca 10 satırları seçmek için modülü. Yalnızca bu bizim giriş bağlantı noktasını kullanarak burada gösterilen SQL sorgusu için veri satırı seçin:

![Giriş bağlantı noktası veri](./media/hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Web hizmeti
Şimdi web hizmetini yayımlamak için kullanılan küçük bir denemeyi çalıştırmak hazır olursunuz.

#### <a name="generate-input-data-for-webservice"></a>Web hizmeti giriş verileri oluşturma
Sıfırıncı bir adım olarak sayısı tablo büyük olduğundan, birkaç satırlık test verilerini almak ve çıktı verilerini buradan sayısı özelliklerle oluşturun. Bu çıktı, burada gösterildiği gibi, WebService için girdi veri biçimi olarak görev yapabilir:

![BDT giriş verileri oluşturma](./media/hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> Giriş veri biçimi için ÇIKTISINI kullanın **sayısı özelliği Oluşturucu** modülü. Bu deneme çalıştıran bittikten sonra çıktısını Kaydet **sayısı özelliği Oluşturucu** modülü bir veri kümesi olarak. Bu veri kümesi, Web hizmeti giriş verileri için kullanılır.
>
>

#### <a name="scoring-experiment-for-publishing-webservice"></a>Yayımlama Web hizmeti için deneme Puanlama
İlk olarak, temel yapı, eğitilen model nesnemizi kabul eden bir **puan modeli** modülüdür ve önceki adımlarda oluşturulan bazı giriş verileri satırını, **sayı korlaizer** modülünü kullanarak kabul eder. Projeye Scored etiketleri ve puan olasılıklar "Kümesindeki sütunları seçme" kullanın.

![Veri kümesindeki Sütunları seçme](./media/hive-criteo-walkthrough/kRHrIbe.png)

Bildirim nasıl **kümesindeki sütunları seçme** modülü, 'veri kümesinden filtreleme' out'için kullanılabilir. İçeriği burada gösterilir:

![Select Columns in Dataset modülü sütun ile filtreleme](./media/hive-criteo-walkthrough/oVUJC9K.png)

Mavi giriş ve çıkış bağlantı noktaları almak için tıklamanız yeterlidir **webservice hazırlama** sağ alt köşede. Bu denemeyi çalıştırmak da sağlar bize web hizmeti yayımlama: tıklayın **WEB hizmeti yayımlama** simgesi en altına doğru gösterilen burada:

![Web hizmeti yayımlama](./media/hive-criteo-walkthrough/WO0nens.png)

Webservice yayımlandığında, bu nedenle görünen bir sayfasına yönlendirilirsiniz:

![Web hizmet Panosu](./media/hive-criteo-walkthrough/YKzxAA5.png)

İki bağlantı için sol taraftaki webservices'a dikkat edin:

* **İstek/yanıt** hizmet (veya RRS) için tek Öngörüler yöneliktir ve bu atölyeyi kullanılan.
* **Toplu iş yürütme** hizmeti (BES) girdi verilerini Azure Blob Depolama alanında bulunan bir tahminde bulunmak amacıyla kullanılan gerektirir ve batch tahminler elde etmek için kullanılır.

Bağlantısına tıklayarak **istek/yanıt** bize sağlıyor bir sayfa önceden tamamlanmış C#, python ve r kodu alır Bu kod, Web hizmeti çağrıları yapmak için kolayca kullanılabilir. Bu sayfadaki API anahtarının kimlik doğrulaması için kullanılması gerekir.

Bu python kod üzerinde Ipython notebook yeni hücreye kopyalamak uygundur.

Python kodu doğru API anahtarına sahip bir segmentini aşağıda verilmiştir.

![Python kodu](./media/hive-criteo-walkthrough/f8N4L4g.png)

Varsayılan API anahtarı, Web hizmeti 'nin API anahtarımız ile değiştirilmiştir. Tıklayarak **çalıştırma** üzerinde bu hücresinde bir Ipython Not Defteri şu yanıtı verir:

![Ipython yanıt](./media/hive-criteo-walkthrough/KSxmia2.png)

Python betiği JSON çerçevesinde istenen iki test örneği için, "puanlanmış Etiketler, puanlanmış olasılıklara" biçimindeki yanıtları geri alırsınız. Bu durumda, varsayılan değerleri önceden tamamlanmış kod (0 tüm sayısal sütunları ve tüm kategorik sütunlar için "value" dizesi) sağladığı seçildi.

Sonuç olarak, izlenecek yol Azure Machine Learning kullanarak büyük ölçekli veri kümesinin nasıl işleneceğini gösterir. Bir terabayt veri ile başlatılan bir tahmin modeli oluşturulan ve bulutta bir web hizmeti olarak dağıttınız.

