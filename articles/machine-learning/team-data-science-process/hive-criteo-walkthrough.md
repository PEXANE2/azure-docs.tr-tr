---
title: 1-TB veri setinde Azure HDInsight Hadoop Cluster'ı kullanma - Ekip Veri Bilimi Süreci
description: Büyük (1 TB) genel kullanılabilir veri kümesini kullanarak bir model oluşturmak ve dağıtmak için hdinsight Hadoop kümesini kullanan uçdan uca bir senaryo için Ekip Veri Bilimi İşlemini kullanma
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
ms.openlocfilehash: 1198d3cc7ccc0013e7c894488027d8e162470247
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677594"
---
# <a name="the-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>Takım Veri Bilimi Süreci iş başında - 1-TB veri setinde Azure HDInsight Hadoop Kümesi kullanma

Bu gözden geçirme, genel kullanıma açık [Criteo](https://labs.criteo.com/downloads/download-terabyte-click-logs/) veri kümelerinden birinden örnek verileri depolamak, keşfetmek, özellik mühendisi ve aşağı yada bir [Azure HDInsight Hadoop kümesiyle](https://azure.microsoft.com/services/hdinsight/) uçtan uca bir senaryoda Ekip Veri Bilimi İşleminin nasıl kullanılacağını gösterir. Bu veriler üzerinde ikili bir sınıflandırma modeli oluşturmak için Azure Machine Learning'i kullanır. Ayrıca, bu modellerden birinin Web hizmeti olarak nasıl yayımlandırılabildiğini de gösterir.

Bu gözden geçirme de sunulan görevleri gerçekleştirmek için bir IPython dizüstü bilgisayar kullanmak da mümkündür. Bu yaklaşımı denemek isteyen [kullanıcılar, Hive ODBC bağlantı konusunu kullanarak Criteo walkthrough'a](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) başvurmalıdır.

## <a name="criteo-dataset-description"></a><a name="dataset"></a>Criteo Dataset Açıklama
Criteo verileri, 4,3 milyardan fazla kayıttan oluşan 370 GB gzip sıkıştırılmış TSV dosyası (~1,3 TB sıkıştırılmamış) olan bir tıklama tahmini veri kümesidir. [Bu Criteo](https://labs.criteo.com/downloads/download-terabyte-click-logs/)tarafından kullanılabilir yapılan tıklama verileri 24 gün alınır. Veri bilimcilerin rahatlığı için, deney yapmak için elimizdeki veriler fermuarsız hale gelmiştir.

Bu veri kümesindeki her kayıt 40 sütun içerir:

* ilk sütun, kullanıcının bir **eklentiyi** tıklatıp tıklatmadığını (değer 1) veya bir (0 değeri) tıklatıp tıklatmadığını belirten bir etiket sütunudur
* sonraki 13 sütun sayısaldır ve
* son 26 kategorik sütunlar vardır

Sütunlar anonimleştirilmiştir ve numaralandırılmış bir dizi ad kullanır: "Col1" (etiket sütunu için) için "Col40" (son kategorik sütun için).

Burada bu veri kümesinden iki gözlem (satır) ilk 20 sütun bir alıntıdır:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb

Bu veri kümesinde hem sayısal hem de kategorik sütunlarda eksik değerler vardır. Eksik değerleri işlemek için basit bir yöntem açıklanmıştır. Verilerin hive tablolarına depolanırken ek ayrıntıları araştırılır.

**Tanım:** *Tıklama oranı (TO):* Bu metrik verilerdeki tıklama yüzdesidir. Bu Criteo veri setinde, TO yaklaşık % 3.3 veya 0.033'tür.

## <a name="examples-of-prediction-tasks"></a><a name="mltasks"></a>Tahmin görevleriörnekleri
Bu gözden geçirme de iki örnek tahmin sorunu ele alınmıştır:

1. **İkili sınıflandırma**: Bir kullanıcının bir eklentiyi tıklatıp tıklatmadığını tahmin eder:

   * Sınıf 0: Tıklama yok
   * Sınıf 1: Tıklayın
2. **Regresyon**: Kullanıcı özelliklerinden bir reklam tıklaması olasılığını tahmin eder.

## <a name="set-up-an-hdinsight-hadoop-cluster-for-data-science"></a><a name="setup"></a>Veri bilimi için bir HDInsight Hadoop kümesi ayarlama
> [!NOTE]
> Bu adım genellikle bir **Yönetici** görevidir.

HDInsight kümeleriyle tahmine dayalı analitik çözümleri oluşturmak için Azure Veri Bilimi ortamınızı üç adımda ayarlayın:

1. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md): Bu depolama hesabı, verileri Azure Blob Depolama'da depolamak için kullanılır. HDInsight kümelerinde kullanılan veriler burada depolanır.
2. [Veri Bilimi için Azure HDInsight Hadoop Kümelerini özelleştirin](customize-hadoop-cluster.md): Bu adım, tüm düğümlere 64 bit Anaconda Python 2.7 yüklü bir Azure HDInsight Hadoop kümesi oluşturur. HDInsight kümesini özelleştirirken tamamlanması gereken iki önemli adım (bu konuda açıklanan) vardır.

   * 1. adımda oluşturulan depolama hesabını oluşturulduğunda HDInsight kümenizle bağlayın. Bu depolama hesabı küme içinde işlenebilen verilere erişmek için kullanılır.
   * Oluşturulduktan sonra kümenin baş düğümüne Uzaktan Erişimi etkinleştirin. Burada belirttiğiniz uzaktan erişim kimlik bilgilerini (küme oluşturmada belirtilen kimlik bilgilerinden farklı olarak) unutmayın: aşağıdaki yordamları tamamlayın.
3. [Bir Azure Machine Learning Studio (klasik) çalışma alanı oluşturun](../studio/create-workspace.md): Bu Azure Machine Learning çalışma alanı, HDInsight kümesinde ilk veri keşfi nden sonra makine öğrenimi modelleri oluşturmak ve örnekleme oluşturmak için kullanılır.

## <a name="get-and-consume-data-from-a-public-source"></a><a name="getdata"></a>Genel bir kaynaktan veri alma ve kullanma
[Criteo](https://labs.criteo.com/downloads/download-terabyte-click-logs/) veri kümesine bağlantıya tıklayarak, kullanım koşullarını kabul ederek ve bir ad vererek erişilebilir. Burada bir anlık görüntü gösterilir:

![Criteo koşullarını kabul et](./media/hive-criteo-walkthrough/hLxfI2E.png)

Veri kümesi ve kullanılabilirliği hakkında daha fazla bilgi için **İndirmeye Devam** et'i tıklatın.

Veriler Azure [blob depolama](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) konumunda bulunur: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "Wasb" Azure Blob Depolama konumu anlamına gelir.

1. Bu Azure blob depolamasındaki veriler, güniçinde günönce günönce günönce günönce üç alt klasörden oluşur.

   1. Alt klasör *ham/sayım/* verilerin ilk 21 gününü\_içerir -\_00 günden 20 güne kadar
   2. Alt klasör *ham/tren/* tek bir günlük veriden oluşur, 21.\_
   3. Alt klasör *ham/test/* iki günlük veri,\_22.\_
2. Ham gzip verileri, NN'nin 00'den 23'e çıktığı day_NN.gz *olarak* ana klasörde de mevcuttur.

Herhangi bir yerel indirme gerektirmeyen bu verilere erişmek, araştırmak ve modellemek için alternatif bir yaklaşım, kovan tabloları oluştururken daha sonra bu izlenebilir likte açıklanır.

## <a name="log-in-to-the-cluster-headnode"></a><a name="login"></a>Küme başlığına giriş yapın
Kümenin headnode oturum açmak için, kümeyi bulmak için [Azure portalını](https://ms.portal.azure.com) kullanın. Soldaki HDInsight fil simgesine tıklayın ve ardından kümenizin adını çift tıklatın. **Yapılandırma** sekmesine gidin, sayfanın altındaki CONNECT simgesine çift tıklayın ve istendiğinde uzaktan erişim kimlik bilgilerinizi girin ve sizi kümenin headnode'una götürün.

Küme başlığına tipik bir ilk giriş şöyle görünür:

![Kümelemek için oturum açma](./media/hive-criteo-walkthrough/Yys9Vvm.png)

Solda veri arama için bizim beygir olan "Hadoop Komuta Hattı". "Hadoop İplik Durumu" ve "Hadoop Ad Düğümü" olmak gibi iki yararlı URL'ye dikkat edin. İplik durumu URL'si iş ilerlemesini gösterir ve ad düğümü URL küme yapılandırması hakkında ayrıntılar verir.

Artık izliğin ilk bölümüne başlamaya hazırsınız: Hive'ı kullanarak veri araştırması ve Azure Machine Learning için veri hazırlama.

## <a name="create-hive-database-and-tables"></a><a name="hive-db-tables"></a>Kovan veritabanı ve tablolar oluşturma
Criteo veri setimiz için Hive tabloları oluşturmak için, baş düğümünün masaüstündeki ***Hadoop Komut Hattı'nı*** açın ve komutu girerek Hive dizinine girin

    cd %hive_home%\bin

> [!NOTE]
> Bu walkthrough tüm Hive komutları çalıştırın/ dizin istemi. Bu, herhangi bir yol sorunları nın otomatik olarak hallinir. "Hive dizin istemi", "Hive kutusu/ dizin istemi" ve "Hadoop Komut Satırı" terimlerini birbirinin yerine kullanabilirsiniz.
>
> [!NOTE]
> Herhangi bir Hive sorgusunu yürütmek için her zaman aşağıdaki komutları kullanabilirsiniz:
>
>        cd %hive_home%\bin
>        hive

Hive REPL bir "kovan >" işareti ile görünür sonra, sadece kesmek ve yürütmek için sorgu yapıştırın.

Aşağıdaki kod bir veritabanı "criteo" oluşturur ve sonra dört tablo oluşturur:

* gün\_00 gün 20\_için inşa sayıları oluşturmak için bir *tablo,*
* 21. günde\_inşa edilen *tren veri seti olarak kullanılmak üzere* bir tablo ve
* *sırasıyla* 22.\_\_

Günlerden biri tatil olduğundan test veri kümesini iki farklı tabloya bölün. Amaç, modelin tıklama hızından tatil ile tatil dışı arasındaki farkları algılayıp algılayamamasını belirlemektir.

Komut dosyası örneği&#95;kovan&#95;&#95;[ve&#95;tables.hql&#95;&#95;&#95;criteo&#95;oluşturmak](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) için burada kolaylık sağlamak için görüntülenir:

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

Azure Blob Depolama (wasb) konumlarını işaret edebilirsiniz, böylece tüm bu tablolar haricidir.

**ANY Hive sorgusu yürütmek için iki yolu vardır:**

* **Hive REPL komut satırı kullanma**: Birincisi bir "kovan" komutu vermek ve Hive REPL komut satırında bir sorgu kopyalayıp yapıştırmak:

        cd %hive_home%\bin
        hive

     Şimdi REPL komut satırında, sorguyu kesme ve yapıştırma bunu yürütür.
* **Sorguları bir dosyaya kaydetme ve komutu yürütme**: İkinci olarak sorguları bir '.hql' dosyasına kaydetmek ([örnek&#95;kovan&#95;&#95;criteo&#95;veritabanı oluşturmak&#95;ve&#95;tablolar.hql)](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)ve sonra sorguyu yürütmek için aşağıdaki komutu vermektir:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Veritabanı nı ve tablo oluşturmayı onaylama
Ardından, Hive bin/ dizin istemiaşağıdaki komutu ile veritabanının oluşturulmasını onaylayın:

        hive -e "show databases;"

Bu verir:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Bu yeni veritabanı, "criteo" oluşturulmasını doğrular.

Hangi tabloların oluşturulduğunu görmek için, hive bin/ dizin isteminden komutu burada vermen yeterlidir:

        hive -e "show tables in criteo;"

Daha sonra aşağıdaki çıktıyı görmeniz gerekir:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="data-exploration-in-hive"></a><a name="exploration"></a>Hive'da veri keşfi
Şimdi Hive bazı temel veri araştırma yapmaya hazırız. Tren ve test veri tablolarında örnek sayısını sayarak başlarsınız.

### <a name="number-of-train-examples"></a>Tren örneklerinin sayısı
Örnek&#95;kovan&#95;sayısı nın içeriği&#95;[tren&#95;tablo&#95;örnekler.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) burada gösterilmiştir:

        SELECT COUNT(*) FROM criteo.criteo_train;

Bu verimleri:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Alternatif olarak, hive bin / dizin istemi aşağıdaki komutu da verebilir:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>İki test veri kümesindeki test örneklerinin sayısı
Şimdi iki test veri kümesindeki örneklerin sayısını sayın. [Örnek&#95;kovan&#95;sayısı&#95;criteo&#95;test&#95;gün&#95;22&#95;tablo&#95;örnekler.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) burada:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Bu verimleri:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Her zamanki gibi, komutu vererek Hive bin/dizin isteminden komutu da arayabilirsiniz:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Son olarak,\_test veri kümesindeki test örneklerinin sayısını 23.

Bunu yapmak için komut gösterilen [(örnek&#95;kovan&#95;sayısı&#95;criteo&#95;test&#95;gün&#95;23&#95;örnekler.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)bakın) benzer:

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Bu verir:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Tren veri kümesinde etiket dağıtımı
Tren veri kümesindeki etiket dağıtımı ilgi çekicidir. Bunu görmek için, örnek&#95;kovan içeriğini göstermek [&#95;criteo&#95;etiket&#95;dağıtım&#95;tren&#95;table.hql:](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql)

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Bu, etiket dağılımını verir:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Pozitif etiketlerin yüzdesi yaklaşık %3,3'tür (orijinal veri kümesiyle tutarlı).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Tren veri setindeki bazı sayısal değişkenlerin histogram dağılımları
Sayısal değişkenlerin dağılımının neye benzediğini\_bulmak için Hive'ın yerel "histogram sayısal" işlevini kullanabilirsiniz. Burada [örnek&#95;hivet&#95;criteo&#95;histogram&#95;sayısal.hql içeriği şunlardır:](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql)

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Bu, aşağıdakileri verir:

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

LATERAL VIEW - Hive'daki patlayabilir kombinasyonu, her zamanki liste yerine SQL benzeri bir çıktı üretmeye hizmet eder. Bu tabloda, ilk sütun depo ortasına, ikincisi ise depo alanı frekansına karşılık gelir.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Tren veri kümesindeki bazı sayısal değişkenlerin yaklaşık yüzdelik dilimleri
Sayısal değişkenlerle de ilgi çekici olan yaklaşık yüzdelik dilimlerin hesaplanmasıdır. Hive yerli "yüzdelik\_yaklaşık" bizim için yapar. [Örnek&#95;kovan&#95;criteo&#95;yaklaşık&#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) içeriği şunlardır:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Bu verimleri:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Yüzdelik dilimlerin dağılımı genellikle herhangi bir sayısal değişkenin histogram dağılımı ile yakından ilişkilidir.

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Tren veri kümesindeki bazı kategorik sütunlar için benzersiz değerlerin sayısını bulma
Veri arama devam, bulmak, bazı kategorik sütunlar için, aldıkları benzersiz değerlerin sayısı. Bunu yapmak için, [kategorigoricals.hql&#95;benzersiz&#95;değerleri&#95;örnek&#95;kovan&#95;criteo](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql)içeriğini göstermek:

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Bu verimleri:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Col15 19M benzersiz değerlere sahiptir! Bu tür yüksek boyutlu kategorik değişkenleri kodlamak için "tek sıcak kodlama" gibi saf teknikler kullanmak mümkün değildir. Özellikle, bu sorunla etkin bir şekilde başa çıkma için [Sayımlarla Öğrenme](https://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) adlı güçlü ve sağlam bir teknik açıklanır ve gösterilmiştir.

Son olarak, diğer bazı kategorik sütunlar için de benzersiz değerlerin sayısına bakın. [Örnek&#95;kovan&#95;criteo&#95;benzersiz&#95;değerleri&#95;birden fazla&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) içeriği şunlardır:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Bu verimleri:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Yine, Col20 dışında, diğer tüm sütunların birçok benzersiz değerlere sahip olduğunu unutmayın.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Tren veri kümesindeki kategorik değişken çiftlerinin birlikte oluşum sayıları

Kategorik değişken çiftlerinin sayım dağılımları da ilgi çekicidir. Bu [örnek&#95;kovan&#95;criteo&#95;kategorik&#95;sayar.hql&#95;eşleştirilmiş](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql)kod kullanılarak belirlenebilir:

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Sayıları oluşlarına göre tersine sırala ve bu durumda ilk 15'e bakın. Bu bize verir:

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

## <a name="down-sample-the-datasets-for-azure-machine-learning"></a><a name="downsample"></a>Azure Machine Learning için veri kümelerini aşağıdan örnekle
Veri kümelerini araştırdıktan ve azure machine learning'deki modellerin oluşturulabilmesi için veri kümelerini örnekleyen herhangi bir değişken (kombinasyonlar dahil) için bu tür bir araştırmanın nasıl yapılacağını gösterdikten sonra, veri kümelerini inceleyebilirsiniz. Sorunun odağının şu olduğunu hatırlayın: örnek öznitelikler kümesi (Col2 - Col40'ın özellik değerleri) göz önüne alındığında, Col1'in 0 (tıklama yok) veya 1 (tıklama) olup olmadığını tahmin edin.

Treni örneklemek ve veri kümelerini orijinal boyutun %1'ine kadar test etmek için Hive'ın yerel RAND() işlevini kullanın. Sonraki komut dosyası, [örnek&#95;kovan&#95;criteo&#95;downsample&#95;tren&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) tren veri seti için bunu yapar:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Bu verimleri:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Komut dosyası [örnek&#95;kovan&#95;criteo&#95;alt örnek&#95;test&#95;gün&#95;22&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) test verileri için yapar, gün\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Bu verimleri:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Son olarak, komut dosyası [örnek&#95;kovan&#95;criteo&#95;downsample&#95;testi&#95;gün&#95;23&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) test verileri için yapar, gün\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Bu verimleri:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Bununla, Azure Machine Learning'de modeller oluşturmak için örneklenmiş tren ve test veri kümelerimizi kullanmaya hazırsınız.

Azure Machine Learning'e geçmeden önce son önemli bir bileşen vardır ve bu bileşen sayım tablosuyla ilgilidir. Bir sonraki alt bölümde, sayım tablosu bazı ayrıntılı olarak ele alınmıştır.

## <a name="a-brief-discussion-on-the-count-table"></a><a name="count"></a>Sayım tablosunda kısa bir tartışma
Gördüğünüz gibi, birkaç kategorik değişkenin yüksek bir boyutsallığı var. Bu değişkenleri verimli ve sağlam bir şekilde kodlamak için [Sayımlarla Öğrenme](https://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) adlı güçlü bir teknik sunulur. Bu teknik hakkında daha fazla bilgi sağlanan bağlantıda yer alan.

>[!NOTE]
>Bu gözden geçirmede, yüksek boyutlu kategorik özelliklerin kompakt temsillerini üretmek için sayım tablolarının kullanılmasına odaklanılır. Kategorik özellikleri kodlamanın tek yolu bu değildir; diğer teknikler hakkında daha fazla bilgi için, ilgilenen kullanıcılar [bir-sıcak kodlama](https://en.wikipedia.org/wiki/One-hot) ve [özellik karma](https://en.wikipedia.org/wiki/Feature_hashing)kontrol edebilirsiniz.
>

Sayım verilerinde sayım tabloları oluşturmak için, ham/sayım klasöründeki verileri kullanın. Modelleme bölümünde, kullanıcılara bu sayım tablolarını sıfırdan kategorik özellikler için nasıl oluşturacakları veya alternatif olarak keşifleri için önceden oluşturulmuş bir sayım tablosunu nasıl kullanacakları gösterilir. Aşağıdaki lerden sonra, "önceden oluşturulmuş sayım tabloları" atıfta bulunulduğunda, sağlanan sayım tablolarını kullanmayı kastediyoruz. Bu tablolara nasıl erişileceklerine ilişkin ayrıntılı talimatlar sonraki bölümde verilmiştir.

## <a name="build-a-model-with-azure-machine-learning"></a><a name="aml"></a>Azure Machine Learning ile bir model oluşturun
Azure Machine Learning'deki model oluşturma sürecimiz aşağıdaki adımları izler:

1. [Hive tablolarından verileri Azure Machine Learning'e taşıyın](#step1)
2. [Denemeyi oluşturun: verileri temizleyin ve sayım tablolarıyla bir özellik haline getirin](#step2)
3. [Modeli oluşturun, eğitin ve puanla](#step3)
4. [Modeli değerlendirme](#step4)
5. [Modeli web hizmeti olarak yayımlama](#step5)

Artık Azure Machine Learning stüdyosunda modeller oluşturmaya hazırsınız. Aşağıda örneklenmiş verilerimiz kümedeki Hive tabloları olarak kaydedilir. Bu verileri okumak için Azure Machine Learning **Alma Verisi** modüllerini kullanın. Bu kümenin depolama hesabına erişmek için kimlik bilgileri aşağıdaki şekilde sağlanır.

### <a name="step-1-get-data-from-hive-tables-into-azure-machine-learning-using-the-import-data-module-and-select-it-for-a-machine-learning-experiment"></a><a name="step1"></a>1. Adım: Alma Verileri modülünü kullanarak Hive tablolarından Azure Machine Learning'e veri alın ve bir makine öğrenme denemesi için seçin
**+Yenİ** -> **DENEY** -> **Boş Deneme**seçerek başlayın. Ardından, sol üstteki **Arama** kutusundan "Verileri Alma"yı arayın. Veri erişimi için modülü kullanmak için Veri **Modülünü** sürükleyin ve deneme tuvaline (ekranın orta kısmı) sürükleyin ve bırakın.

Hive tablosundan veri alırken **Alma Verileri** aşağıdaki gibidir:

![Veri Alma veri alır](./media/hive-criteo-walkthrough/i3zRaoj.png)

Alma **Verileri** modülü için, grafikte sağlanan parametrelerin değerleri, sağlamanız gereken değerlerin yalnızca örnekleridir. Burada, **İçe Aktar Veri** modülü için parametre kümesinin nasıl doldurulabildiğini anlatan bazı genel kılavuzlar verem.

1. **Veri Kaynağı** için "Kovan sorgusu" seçeneğini belirleyin
2. **Hive veritabanı sorgu** kutusunda, basit bir SELECT\_\_* FROM\_\_<veritabanı adınızı.tablo adınız> - yeterlidir.
3. **Hcatalog server URI**: Kümeniz "abc" ise, o\/zaman bu basitçe: https: /abc.azurehdinsight.net
4. **Hadoop kullanıcı hesabı adı**: Kümenin devreye alınması sırasında seçilen kullanıcı adı. (NOT Uzaktan Erişim kullanıcı adı!)
5. **Hadoop kullanıcı hesabı şifresi**: Kümenin devreye alınması sırasında seçilen kullanıcı adının şifresi. (NOT Uzaktan Erişim şifresi!)
6. **Çıktı verilerinin konumu**: "Azure"u seçin
7. **Azure Depolama hesap adı**: Kümeyle ilişkili depolama hesabı
8. **Azure Depolama hesap anahtarı**: Kümeyle ilişkili depolama hesabının anahtarı.
9. **Azure kapsayıcı adı**: Küme adı "abc" ise, bu genellikle "abc"dir.

**İçe Aktarma Verileri** veri almayı tamamladıktan sonra (Modül'deki yeşil onay işaretini görürsünüz), bu verileri bir Veri Kümesi olarak kaydedin (seçtiğiniz bir adla). Bu neye benziyor:

![Veri kaydet veri kaydet](./media/hive-criteo-walkthrough/oxM73Np.png)

**İçe Aktarma Verimodülünün** çıkış bağlantı noktasına sağ tıklayın. Bu, **veri kümesi olarak Kaydet** seçeneğini ve **Visualize** seçeneğini ortaya çıkarır. Tıklatıldığında **Görselleştir** seçeneği, bazı özet istatistikleri için yararlı olan sağ panelle birlikte 100 veri satırı görüntüler. Veri kaydetmek için **veri seti olarak Kaydet'i** seçin ve yönergeleri izleyin.

Makine öğrenimi deneyinde kullanılmak üzere kaydedilen veri kümesini seçmek için, aşağıdaki şekilde gösterilen **Arama** kutusunu kullanarak veri kümelerini bulun. Ardından, veri kümesine erişmek için kısmen vermiş olduğunuz adı yazın ve veri kümesini ana panele sürükleyin. Ana panel üzerine bırakarak makine öğrenme modelleme kullanılmak üzere seçer.

![Dataset'i ana panele sürükleyin](./media/hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> Bunu hem tren hem de test veri kümeleri için yapın. Ayrıca, bu amaçla vermiş olduğunuz veritabanı adını ve tablo adlarını kullanmayı unutmayın. Şekilde kullanılan değerler yalnızca illüstrasyon amaçlıdır.**
>
>

### <a name="step-2-create-an-experiment-in-azure-machine-learning-to-predict-clicks--no-clicks"></a><a name="step2"></a>Adım 2: Tıklamaları / tıklama yok tahmin etmek için Azure Machine Learning'de deneme oluşturma
Azure Machine Learning Studio (klasik) denememiz şuna benzer:

![Makine Öğrenimi deneyi](./media/hive-criteo-walkthrough/xRpVfrY.png)

Şimdi bu deneyin temel bileşenlerini inceleyin. Kaydedilmiş trenimizi sürükleyin ve veri kümelerini önce deneme tuvalimize sürükleyin.

#### <a name="clean-missing-data"></a>Eksik Verileri Temizleme
**Temiz Eksik Veri** modülü adının önerdiği şeyi yapar: eksik verileri kullanıcı tarafından belirtilen şekillerde temizler. Bunu görmek için bu modüle bakın:

![Eksik verileri temizleme](./media/hive-criteo-walkthrough/0ycXod6.png)

Burada, tüm eksik değerleri 0 ile değiştirmeyi seçin. Modüldeki açılır düşüşlere bakarak görülebilecek başka seçenekler de vardır.

#### <a name="feature-engineering-on-the-data"></a>Veriler üzerinde özellik mühendisliği
Büyük veri kümelerinin bazı kategorik özellikleri için milyonlarca benzersiz değer olabilir. Bu tür yüksek boyutlu kategorik özellikleri temsil etmek için tek sıcak kodlama gibi saf yöntemler kullanmak tamamen mümkün değildir. Bu gözden geçirme, bu yüksek boyutlu kategorik değişkenlerin kompakt gösterimlerini oluşturmak için yerleşik Azure Machine Learning modüllerini kullanarak sayım özelliklerinin nasıl kullanılacağını gösterir. Sonuç, daha küçük bir model boyutu, daha hızlı eğitim süreleri ve diğer tekniklerle karşılaştırılabilecek performans ölçümleridir.

##### <a name="building-counting-transforms"></a>Bina sayma dönüşümleri
Sayım özellikleri oluşturmak için Azure Machine Learning'de kullanılabilen **Yapı Sayımı Dönüşümü** modüllerini kullanın. Modül şuna benzer:

![Yapı Sayma](./media/hive-criteo-walkthrough/e0eqKtZ.png)
![Dönüşümü modülü özellikleri Yapı Sayma Dönüşümü modülü](./media/hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT]
> Sayım **sütunları** kutusuna, sayım yapmak istediğiniz sütunları girin. Genellikle, bunlar (belirtildiği gibi) yüksek boyutlu kategorik sütunlardır. Criteo veri kümesinin Col15'ten Col40'a kadar 26 kategorik sütunu olduğunu unutmayın. Burada, hepsine güvenin ve endekslerini verin (gösterildiği gibi virgülle ayrılmış 15 ila 40 arası).
>

Modülü MapReduce modunda (büyük veri kümeleri için uygun) kullanmak için, bir HDInsight Hadoop kümesine (özellik aramaiçin kullanılan kümeye ve kimlik bilgilerine de yeniden kullanılabilir) erişmeniz gerekir. Önceki rakamlar, doldurulmuş değerlerin nasıl göründüğünü göstermektedir (çizim için sağlanan değerleri kendi kullanım örneğinizle alakalı olanlarla değiştirin).

![Modül parametreleri](./media/hive-criteo-walkthrough/05IqySf.png)

Önceki şekil, giriş blob konumunun nasıl girilen gösterir. Bu konum, bina sayısı tabloları için ayrılmış verilere sahiptir.

Bu modül çalışma bittiğinde, modülü sağ tıklayarak ve **Dönüştür olarak Kaydet** seçeneğini seçerek dönüşümü daha sonraya kaydedin:

!["Dönüştür olarak kaydet" seçeneği](./media/hive-criteo-walkthrough/IcVgvHR.png)

Yukarıda gösterilen deneme mimarimizde, "ytransform2" veri kümesi tam olarak kaydedilmiş bir sayı dönüşümüne karşılık gelir. Bu denemenin geri kalanı için, okuyucunun sayımoluşturmak için bazı verilerde **Yapı Sayma Dönüşümü** modülü kullandığı ve daha sonra bu sayımları tren ve test veri kümelerinde sayım özellikleri oluşturmak için kullanabileceği varsayılır.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Tren ve test veri kümelerinin bir parçası olarak hangi sayım özelliklerinin dahil edilemesini seçme
Sayım hazır olduktan sonra, kullanıcı **Değişiklik Sayısı Tablo Parametreleri** modüllerini kullanarak tren ve test veri kümelerine hangi özellikleri ekeceğini seçebilir. Bütünlük için, bu modül burada gösterilir. Ama basitlik çıkarları için aslında bizim deney de kullanmayın.

![Sayım Tablosu parametrelerini değiştirin](./media/hive-criteo-walkthrough/PfCHkVg.png)

Bu durumda, görüldüğü gibi, günlük oran kullanılacak ve geri sütun yoksayılır. Ayrıca çöp kutusu eşiği, yumuşatma için kaç sözde önceki örnek eklemek ve herhangi bir Laplacian gürültü kullanıp kullanmayacağınız gibi parametreleri ayarlayabilirsiniz. Tüm bunlar gelişmiş özelliklerdir ve varsayılan değerlerin bu tür özellik oluşturmada yeni olan kullanıcılar için iyi bir başlangıç noktası olduğu unutulmamalıdır.

##### <a name="data-transformation-before-generating-the-count-features"></a>Sayım özelliklerini oluşturmadan önce veri dönüşümü
Şimdi odak aslında sayı özellikleri üreten önce bizim tren ve test verileri dönüştürme hakkında önemli bir nokta üzerindedir. Sayım dönüşümü verilerimize uygulanmadan önce kullanılan iki **Execute R Script** modülü vardır.

![R Script modüllerini çalıştırın](./media/hive-criteo-walkthrough/aF59wbc.png)

İşte ilk R komut dosyası:

![İlk R komut dosyası](./media/hive-criteo-walkthrough/3hkIoMx.png)

Bu R komut dosyası sütunlarımızı "Col1" ile "Col40" adlarıyla yeniden adlandırır. Bunun nedeni, sayı dönüştürmenin bu biçimin adlarını beklemesidir.

İkinci R komut dosyası, negatif sınıfı aşağı örnekleme yaparak pozitif ve negatif sınıflar arasındaki dağılımı (sırasıyla 1 ve 0 sınıfı) dengeler. R komut dosyası burada bunu nasıl yapacağını gösterir:

![İkinci R komut dosyası](./media/hive-criteo-walkthrough/91wvcwN.png)

Bu basit R komut dosyasında, pozitif ve negatif sınıflar arasındaki denge miktarını ayarlamak için "pos\_neg\_oranı" kullanılır. Sınıf dengesizliğinin iyileştirilmesi genellikle sınıf dağılımının çarpık olduğu sınıflandırma problemleri için performans avantajları na sahip olduğundan bunu yapmak önemlidir (bu durumda %3,3 pozitif sınıfa ve %96,7 negatif sınıfa sahip olduğunuzu hatırlayın).

##### <a name="applying-the-count-transformation-on-our-data"></a>Verilerimizde sayım dönüşümlerini uygulama
Son olarak, dönüşüm uygulama **modüllerini** kullanarak tren ve test veri kümelerimize sayım dönüşümlerini uygulayabilirsiniz. Bu modül, kaydedilen sayı dönüşümlerini bir giriş olarak, tren veya test veri kümelerini diğer giriş olarak alır ve sayım özellikleriyle verileri döndürür. Burada gösterilmiştir:

![Dönüşüm modüllerini uygula](./media/hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Sayım özelliklerinin nasıl göründüğüne bir alıntı
Bizim durumumuzda sayım özelliklerinin nasıl göründüğünü görmek öğreticidir. İşte bu bir alıntıdır:

![Sayma özellikleri](./media/hive-criteo-walkthrough/FO1nNfw.png)

Bu alıntı, sayılan sütunlar için, ilgili geri dönüşlere ek olarak sayımları ve günlük oranlarını elde ettiğinizi gösterir.

Artık bu dönüştürülmüş veri kümelerini kullanarak bir Azure Machine Learning modeli oluşturmaya hazırsınız. Sonraki bölümde bu nasıl yapılabilir gösterir.

### <a name="step-3-build-train-and-score-the-model"></a><a name="step3"></a>Adım 3: Modeli oluşturun, eğitin ve puanla

#### <a name="choice-of-learner"></a>Öğrenci seçimi
İlk olarak, bir öğrenci seçmeniz gerekir. Öğrencimiz olarak iki sınıflı güçlendirilmiş karar ağacı kullanın. Bu öğrenci için varsayılan seçenekler şunlardır:

![İki Sınıf Artırılmış Karar Ağacı parametreleri](./media/hive-criteo-walkthrough/bH3ST2z.png)

Deneme için varsayılan değerleri seçin. Varsayılanlar anlamlıdır ve performansa hızlı taban çizgileri elde etmenin iyi bir yoludur. Bir taban çizgisine sahip olduktan sonra seçerseniz, parametreleri süpürerek performansı artırabilirsiniz.

#### <a name="train-the-model"></a>Modeli eğitme
Eğitim için, sadece bir **Tren Modeli** modülü çağırmak. Bunun iki girişi Iki Sınıf Artırılmış Karar Ağacı öğrenen ve bizim tren veri kümesi vardır. Bu burada gösterilmiştir:

![Tren Modeli modülü](./media/hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-the-model"></a>Modeli puanlama
Eğitimli bir modele sahip olduktan sonra, test veri setinde puan almaya ve performansını değerlendirmeye hazırsınız. Bunu, aşağıdaki şekilde gösterilen **Puan Modeli** modülünü ve Model **Değerlendir** modülünü kullanarak yapın:

![Score Model (Model Puanlama) modülü](./media/hive-criteo-walkthrough/fydcv6u.png)

### <a name="step-4-evaluate-the-model"></a><a name="step4"></a>Adım 4: Modeli değerlendirin
Son olarak, model performansını çözümlemelisiniz. Genellikle, iki sınıf (ikili) sınıflandırma sorunları için, iyi bir ölçü AUC olduğunu. Bu eğriyi görselleştirmek **için, Score Model** modülünü Bir **Model Değerlendir** modülüne bağlayın. **Modeli Değerlendir** modülünde **Görselleştir'i** tıklattığınızda aşağıdaki gibi bir grafik verir:

![Modül BDT modelini değerlendirin](./media/hive-criteo-walkthrough/0Tl0cdg.png)

İkili (veya iki sınıf) sınıflandırma problemlerinde, tahmin doğruluğunun iyi bir ölçüsü Eğrinin Altındaki Alandır (AUC). Aşağıdaki bölümde, test veri setimizde bu modeli kullanarak sonuçlarımızı gösterilmektedir. **Model Değerlendir** modülünün çıkış bağlantı noktasını sağ tıklatın ve ardından **Görselleştir.**

![Model modüllerini değerlendirin](./media/hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step-5-publish-the-model-as-a-web-service"></a><a name="step5"></a>Adım 5: Modeli Web hizmeti olarak yayımlama
Azure Machine Learning modelini web hizmetleri olarak en az yaygara yla yayımlayabilme özelliği, bu modeli yaygın olarak kullanılabilir hale getirmek için değerli bir özelliktir. Bu yapıldıktan sonra, herkes için öngörüler gereken giriş verileri ile web hizmetine arama yapabilir ve web hizmeti bu tahminleri döndürmek için modeli kullanır.

İlk olarak, **Tren Modeli** modülüne sağ tıklayarak ve Eğitilmiş Model Olarak Kaydet seçeneğini kullanarak eğitimli modelimizi Eğitilmiş Model nesnesi olarak **kaydedin.**

Ardından, web hizmetimiz için giriş ve çıkış bağlantı noktaları oluşturun:

* bir giriş bağlantı noktası, tahmin için ihtiyaç duyduğunuz verilerle aynı formda veri alır
* bir çıkış bağlantı noktası Puanlı Etiketleri ve ilişkili olasılıkları döndürür.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Giriş bağlantı noktası için birkaç veri satırı seçin
Giriş bağlantı noktası verileri olarak hizmet vermek için yalnızca 10 satır seçmek için SQL **Dönüşümü Uygula** modülünü kullanmak uygundur. Burada gösterilen SQL sorgusunu kullanarak giriş bağlantı noktasımız için yalnızca bu veri satırlarını seçin:

![Giriş noktası verileri](./media/hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Web hizmeti
Artık web hizmetimizi yayınlamak için kullanılabilecek küçük bir deneme yürütmeye hazırsınız.

#### <a name="generate-input-data-for-webservice"></a>Web hizmeti için giriş verileri oluşturma
Sıfırıncı bir adım olarak, sayım tablosu büyük olduğundan, birkaç test verisi satırı alın ve sayım özellikleriyle ondan çıktı verileri oluşturun. Bu çıktı, burada gösterildiği gibi, web hizmetimiz için giriş veri biçimi olarak hizmet verebilir:

![BDT giriş verileri oluşturma](./media/hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> Giriş veri formatı için **Count Featurizer** modülünün OUTPUT'ını kullanın. Bu deneme çalışma tamamlandığında, veri kümesi olarak **Count Featurizer** modülünden çıktı kaydedin. Bu Dataset, web hizmetindeki giriş verileri için kullanılır.
>
>

#### <a name="scoring-experiment-for-publishing-webservice"></a>Web hizmeti yayınlamak için puanlama denemesi
İlk olarak, temel yapı, eğitimli model nesnemizi kabul eden bir **Puan Modeli** modülü ve **Count Featurizer** modülü kullanılarak önceki adımlarda oluşturulan birkaç giriş verihattıdır. Puanlı etiketleri ve Puan olasılıklarını yansıtmak için "Veri Kümesi'nde Sütun seç" seçeneğini kullanın.

![Veri kümesindeki Sütunları seçme](./media/hive-criteo-walkthrough/kRHrIbe.png)

**Dataset modülündeki Sütunları Seç** modülünün verileri veri kümesinden 'filtreleme' için nasıl kullanılabileceğini öğrenin. İçeriği burada gösterilmiştir:

![Dataset modülündeki Sütunları Seç ile filtreleme](./media/hive-criteo-walkthrough/oVUJC9K.png)

Mavi giriş ve çıkış bağlantı noktalarını almak için sağ alttaki **webservisini hazırlama'ya** tıklamanız yeterlidir. Bu denemeyi çalıştırmak aynı zamanda web hizmetini yayınlamamıza da olanak sağlar: sağ alttaki **WEB HİzMETİ** SATIN ALINMASıNı tıklayın, burada gösterilmiştir:

![Web hizmetini yayımla](./media/hive-criteo-walkthrough/WO0nens.png)

Web hizmeti yayımlandıktan sonra, aşağıdaki gibi görünen bir sayfaya yönlendirilir:

![Web hizmeti panosu](./media/hive-criteo-walkthrough/YKzxAA5.png)

Sol tarafta ki web hizmetleri için iki bağlantıya dikkat edin:

* **İsteK/YANIT HİzMETİ** (veya RRS) tek tahminler içindir ve bu atölyede kullanılan şeydir.
* **TOPLU YÜRÜTME** Hizmeti (BES), toplu iş tahminleri için kullanılır ve öngörülerde bulunmak için kullanılan giriş verilerinin Azure Blob Depolama'da bulunmasını gerektirir.

**Request/RESPONSE** bağlantısına tıkladığınızda bizi C#, python ve R'de önceden konserve kod veren bir sayfaya götürür. Bu kod, web hizmetine çağrı yapmak için rahatlıkla kullanılabilir. Bu sayfadaki API anahtarının kimlik doğrulaması için kullanılması gerekir.

Bu python kodunu IPython not defterindeki yeni bir hücreye kopyalamak uygundur.

Burada python kodunun doğru API anahtarına sahip bir kesimi vermiştir.

![Python kodu](./media/hive-criteo-walkthrough/f8N4L4g.png)

Varsayılan API anahtarı web hizmetimizin API anahtarıyla değiştirildi. IPython not defterinde bu hücrede **Çalıştır'ı** tıklattığınızda aşağıdaki yanıt verir:

![IPython yanıtı](./media/hive-criteo-walkthrough/KSxmia2.png)

Python komut dosyası JSON çerçevesinde sorulan iki test örneği için yanıtları "Puanlı Etiketler, Puanlı Olasılıklar" şeklinde geri alırsınız. Bu durumda, önceden konserve kodun sağladığı varsayılan değerler seçilmiştir (tüm sayısal sütunlar için 0'lar ve tüm kategorik sütunlar için "değer" dizesi).

Sonuç olarak, gözden geçirme miz Azure Machine Learning'i kullanarak büyük ölçekli veri kümesinin nasıl işleyeceğini gösterir. Bir terabaytlık veriyle başladınız, bir tahmin modeli inşa edin ve bulutta bir web hizmeti olarak dağıttınız.

