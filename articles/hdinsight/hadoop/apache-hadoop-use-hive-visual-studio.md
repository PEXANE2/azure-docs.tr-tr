---
title: Visual Studio için Apache Hive & Data Lake araçları - Azure HDInsight
description: Azure HDInsight'ta Apache Hadoop ile Apache Hive sorgularını çalıştırmak için Visual Studio için Veri Gölü araçlarını nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/27/2019
ms.openlocfilehash: 27ab13481525819eb1435f4c9ac256a21acd21fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687797"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçlarını kullanarak Apache Hive sorgularını çalıştırma

Apache Hive'ı sorgulamak için Visual Studio için Veri Gölü araçlarını nasıl kullanacağınızı öğrenin. Veri Gölü araçları, Azure HDInsight'ta Apache Hadoop'a Hive sorgularını kolayca oluşturmanıza, göndermenize ve izlemenize olanak tanır.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Hadoop kümesi. Bu öğeyi oluşturma hakkında daha fazla bilgi için, [Kaynak Yöneticisi şablonu kullanarak Azure HDInsight'ta Apache Hadoop kümesi Oluştur'a](./apache-hadoop-linux-tutorial-get-started.md)bakın.

* [Görsel Stüdyo](https://visualstudio.microsoft.com/vs/). Bu makaledeki adımlar Visual Studio 2019'u kullansın.

* Visual Studio için HDInsight araçları veya Visual Studio için Azure Veri Gölü araçları. Araçları yükleme ve yapılandırma hakkında daha fazla bilgi için [Visual Studio için Veri Gölü Araçlarını Yükle'ye](apache-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)bakın.

## <a name="run-apache-hive-queries-using-the-visual-studio"></a>Visual Studio'yu kullanarak Apache Hive sorgularını çalıştırın

Hive sorguları oluşturmak ve çalıştırmak için iki seçeneğiniz vardır:

* Geçici sorgular oluşturun.
* Bir Hive uygulaması oluşturun.

### <a name="create-an-ad-hoc-hive-query"></a>Geçici hive sorgusu oluşturma

Geçici sorgular **Toplu veya** **Etkileşimli** modda yürütülebilir.

1. **Visual Studio'u** başlatın ve **kodsuz Devam'ı**seçin.

2. **Server Explorer'dan** **Azure'a**sağ tıklayın, Microsoft **Azure Aboneliğine Bağlan'ı seçin...** ve oturum açma işlemini tamamlayın.

3. **HDInsight'ı**genişletin, sorguyu çalıştırmak istediğiniz kümeyi sağ tıklatın ve ardından **Kovan Sorgusu Yaz'ı**seçin.

4. Aşağıdaki kovan sorgusunu girin:

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. **Yürüt**’ü seçin. Yürütme modu **Interactive**varsayılan.

    ![İnteraktif Kovan sorgusu, Visual Studio yürüt](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. Aynı sorguyu **Toplu İşlem** modunda çalıştırmak için, açılan listeyi **Interactive'den** **Toplu İşleme'ye**geçin. Yürütme düğmesi **Yürüt'ten** **Gönder'e**değişir.

    ![Toplu Kovan sorgusu gönder, Visual Studio](./media/apache-hadoop-use-hive-visual-studio/visual-studio-batch-query.png)

    Hive düzenleyicisi IntelliSense’i destekler. Visual Studio için Data Lake Araçları, Hive betiğinizi düzenlerken uzak meta verilerin yüklenmesini destekler. Örneğin, yazarsanız, `SELECT * FROM`IntelliSense önerilen tüm tablo adlarını listeler. Bir tablo adı belirtildiğinde, IntelliSense sütun adlarını listeler. Araçlar çoğu Hive DML deyimlerini, alt sorguları ve yerleşik UDF'leri destekler. IntelliSense yalnızca HDInsight araç çubuğunda seçilen kümelerin meta verilerini önerir.

7. Sorgu araç çubuğunda (sorgu sekmesinin altındaki ve sorgu metninin üzerindeki alan) Gönder'i **seçin**veya **Gönder'in** yanındaki çekme oku'nu seçin ve geri çekme listesinden **Gelişmiş'i** seçin. İkinci seçeneği seçerseniz,

8. Gelişmiş gönder seçeneğini seçtiyseniz, **Komut Dosyası Gönder** iletişim kutusunda İş **Adı,** Bağımsız **Değişkenler, Ek Yapılandırmalar**ve **Durum Dizini'ni** yapılandırın. **Arguments** Ardından **Gönder'i**seçin.

    ![Script iletişim kutusu, HDInsight Hadoop Kovan sorgusu gönder](./media/apache-hadoop-use-hive-visual-studio/vs-tools-submit-jobs-advanced.png)

### <a name="create-a-hive-application"></a>Hive uygulaması oluşturma

Hive uygulaması oluşturarak bir Hive sorgusu çalıştırmak için aşağıdaki adımları izleyin:

1. Açık **Görsel Studio**.

2. **Başlat** penceresinde yeni **bir proje oluştur'u**seçin.

3. Yeni **proje oluşturma** penceresinde, **şablon ara** *kutusunda, Kovan'ı*girin. Ardından **Kovan Uygulaması'nı** seçin ve **İleri'yi**seçin.

4. Yeni **proje pencerenizi Yapılandır' da,** **proje adını**girin, yeni proje **için** konum seçin veya oluşturun ve sonra **Oluştur'u**seçin.

5. Bu projeyle oluşturulan **Script.hql** dosyasını açın ve aşağıdaki HiveQL deyimlerini yapıştırın:

    ```hql
    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Bu ifadeler aşağıdaki eylemleri yapmak:

    * `DROP TABLE`: Varsa tabloyu siler.

    * `CREATE EXTERNAL TABLE`: Hive'da yeni bir 'harici' tablo oluşturur. Dış tablolar yalnızca Hive tablo tanımını depolar. (Veriler özgün konumda bırakılır.)

        > [!NOTE]  
        > Temel verilerin MapReduce işi veya Azure hizmeti gibi harici bir kaynak tarafından güncelleştirilmesini beklediğiniz zaman harici tablolar kullanılmalıdır.
        >
        > Harici bir tablonun düşmesi verileri **silmez,** yalnızca tablo tanımını siler.

    * `ROW FORMAT`: Hive'a verilerin nasıl biçimlendiğini söyler. Bu durumda, her günlükteki alanlar bir boşlukla ayrılır.

    * `STORED AS TEXTFILE LOCATION`: Hive'a verilerin *örnek/veri* dizininde depolanır ve metin olarak depolanır.

    * `SELECT`: Sütunun `t4` değeri `[ERROR]`içerdiği tüm satırların sayısını seçer. Üç satır bu `3`değeri içerdiğinden, bu ifade nin bir değerini döndürür.

    * `INPUT__FILE__NAME LIKE '%.log'`: Hive'a yalnızca .log ile biten dosyalardan veri döndürmesini söyler. Bu yan tümce, aramayı verileri içeren *sample.log* dosyasıyla sınırlandırıyor.

6. Sorgu dosyası araç çubuğundan (geçici sorgu araç çubuğuna benzer bir görünüme sahip), bu sorgu için kullanmak istediğiniz HDInsight kümesini seçin. Ardından **Etkileşimli'yi** **Toplu İşlem** olarak değiştirin (gerekirse) ve deyimleri kovan işi olarak çalıştırmak için **Gönder'i** seçin.

   **Kovan İş Özeti** görüntülenir ve çalışan iş hakkında bilgi görüntüler. **İş Durumu** **Tamamlanana**göre değişene kadar iş bilgilerini yenilemek için **Yenile** bağlantısını kullanın.

   ![Tamamlanmış Kovan iş özeti, Kovan uygulaması, Visual Studio](./media/apache-hadoop-use-hive-visual-studio/hdinsight-job-summary.png)

7. Bu işin çıktısını görüntülemek için **İş Çıktısı'nı** seçin. Bu `[ERROR] 3`sorgu tarafından döndürülen değer , görüntüler.

### <a name="additional-example"></a>Ek örnek

Aşağıdaki örnek, önceki `log4jLogs` yordamda oluşturulan tabloya dayanır, [Bir Kovan uygulaması oluşturun.](#create-a-hive-application)

1. **Server Explorer'dan**cluster'ınızı sağ tıklatın ve **Kovan Sorgu yaz'ı**seçin.

2. Aşağıdaki kovan sorgusunu girin:

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Bu ifadeler aşağıdaki eylemleri yapmak:

    * `CREATE TABLE IF NOT EXISTS`: Zaten yoksa bir tablo oluşturur. `EXTERNAL` Anahtar kelime kullanılmadığından, bu deyim bir iç tablo oluşturur. Dahili tablolar Hive veri ambarında depolanır ve Hive tarafından yönetilir.

        > [!NOTE]  
        > Tabloların aksine, `EXTERNAL` dahili tablonun düşmesi de temel verileri siler.

    * `STORED AS ORC`: Verileri *en iyi duruma getirilmiş satır sütun (ORC)* biçiminde saklar. ORC, Kovan verilerini depolamak için son derece optimize edilmiş ve verimli bir biçimdir.

    * `INSERT OVERWRITE ... SELECT`: Tablodan `log4jLogs` satırları seçer, `[ERROR]`sonra tabloya `errorLogs` veri ekler.

3. **Gerekirse Etkileşimli'yi** **Toplu İşlem** olarak değiştirin ve **ardından Gönder'i**seçin.

4. İşin tabloyu oluşturduğunu doğrulamak için **Server Explorer'a** gidin ve **Azure** > **HDInsight'ı genişletin.** HDInsight kümenizi genişletin ve ardından **Kovan Veritabanları** > **varsayılan ını**genişletin. **ErrorLogs** tablosu ve **log4jLogs** tablosu listelenir.

## <a name="next-steps"></a>Sonraki adımlar

Gördüğünüz gibi, Visual Studio için HDInsight araçları, HDInsight'ta Hive sorgularıyla çalışmak için kolay bir yol sağlar.

* HDInsight'ta Hive hakkında genel bilgi için [Azure HDInsight'ta Apache Hive ve HiveQL nedir?](hdinsight-use-hive.md)

* HDInsight'ta Hadoop ile çalışabileceğiniz diğer yollar hakkında bilgi için, [HDInsight'ta Apache Hadoop'ta MapReduce'i kullanın](hdinsight-use-mapreduce.md)

* Visual Studio için HDInsight araçları hakkında daha fazla bilgi için Azure[HDInsight'a bağlanmak ve Apache Hive sorgularını çalıştırmak için Visual Studio için Veri Gölü Araçlarını Kullan'a](apache-hadoop-visual-studio-tools-get-started.md) bakın
