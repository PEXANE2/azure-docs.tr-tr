---
title: Visual Studio için Data Lake araçları ile Apache Hive-Azure HDInsight
description: Azure HDInsight üzerinde Apache Hadoop ile Apache Hive sorguları çalıştırmak üzere Visual Studio için Data Lake araçları 'nı nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: hrasheed
ms.openlocfilehash: 129f200bc9f61d70f4403b1154978d57e09fee26
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70917502"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Visual Studio Data Lake araçlarını kullanarak Apache Hive sorguları çalıştırma

Apache Hive sorgulamak için Visual Studio Data Lake araçları 'nı nasıl kullanacağınızı öğrenin. Data Lake araçları, Azure HDInsight üzerinde Apache Hadoop için Hive sorgularını kolayca oluşturmanıza, göndermenize ve izlemenize olanak tanır.

## <a id="prereq"></a>Önkoşullar

* HDInsight üzerinde bir Apache Hadoop kümesi. Bkz. [Linux 'Ta HDInsight kullanmaya başlama](./apache-hadoop-linux-tutorial-get-started.md).

* Visual Studio (aşağıdaki sürümlerinden biri):

    * Visual Studio 2015, 2017 (herhangi bir sürüm)
    * Güncelleştirme 4 ile topluluk/profesyonel/Premium/Ultimate Visual Studio 2013

* Visual Studio için HDInsight araçları veya Visual Studio için Azure Data Lake araçları. Araçları yükleme ve yapılandırma hakkında bilgi için bkz. [HDInsight Için Visual Studio Hadoop araçlarını kullanmaya başlama](apache-hadoop-visual-studio-tools-get-started.md) .

## <a id="run"></a>Visual Studio 'Yu kullanarak Apache Hive sorguları çalıştırma

Hive sorguları oluşturmak ve çalıştırmak için iki seçeneğiniz vardır:

* Geçici sorgular oluşturma
* Hive uygulaması oluşturma

### <a name="ad-hoc"></a>Geçici

Geçici sorgular **toplu** veya **etkileşimli** modda çalıştırılabilir.

1. **Visual Studio 'yu**açın.

2. **Sunucu Gezgini** **Azure** > **HDInsight**' a gidin.

3. **HDInsight**' ı genişletin ve sorguyu çalıştırmak istediğiniz kümeye sağ tıklayın ve ardından **Hive sorgusu yaz**' ı seçin.

4. Aşağıdaki Hive sorgusunu girin:

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. **Yürüt**’ü seçin. Yürütme modunun varsayılan olarak **etkileşimli**olduğunu unutmayın.

    ![Etkileşimli Hive sorguları yürütme ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. **Toplu iş** modunda aynı sorguyu çalıştırmak için açılan listeyi **etkileşimli** moddan **Toplu işe**değiştirin. Yürütme düğmesinin **Execute** 'tan **Gönder**'e değiştiği unutulmamalıdır.

    ![Hive sorgusu gönderme ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/visual-studio-batch-query.png)

    Hive düzenleyicisi IntelliSense’i destekler. Visual Studio için Data Lake Araçları, Hive betiğinizi düzenlerken uzak meta verilerin yüklenmesini destekler. Örneğin, yazarsanız `SELECT * FROM`, IntelliSense önerilen tüm tablo adlarını listeler. Bir tablo adı belirtildiğinde, IntelliSense sütun adlarını listeler. Araçlar çoğu Hive DML deyimlerini, alt sorguları ve yerleşik UDF'leri destekler. IntelliSense yalnızca HDInsight araç çubuğunda seçilen kümelerin meta verilerini önerir.

    ![HDInsight Visual Studio Araçları IntelliSense örnek 1’in ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-table-name.png "U-SQL IntelliSense")
   
    ![HDInsight Visual Studio Araçları IntelliSense örnek 2’nin ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-column-name.png "U-SQL IntelliSense")

7. **Gönder** veya **Gönder (Gelişmiş)** öğesini seçin.

   Gelişmiş gönderme seçeneğini belirlerseniz, betik için **İş Adı**, **Bağımsız Değişkenler**, **Ek Yapılandırmalar** ve **Durum Dizini**’ni yapılandırın:

    ![HDInsight Hadoop Hive sorgusunun ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/vs-tools-submit-jobs-advanced.png "Sorgu gönderme")

### <a name="hive-application"></a>Hive uygulaması

1. **Visual Studio 'yu**açın.

2. Menü çubuğundan **Dosya** > **Yeni** > **Proje**' ye gidin.

3. **Yeni proje** penceresinde **Şablonlar** >  > **Azure Data Lake** > **Hive (HDInsight)** **Hive uygulaması**' na gidin. 

4. Bu proje için bir ad girip **Tamam**' ı seçin.

5. Bu projeyle oluşturulan **Script. HQL** dosyasını açın ve aşağıdaki HiveQL deyimlerine yapıştırın:

    ```hiveql
    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Bu deyimler aşağıdaki eylemleri gerçekleştirir:

   * `DROP TABLE`: Tablo varsa, bu ifade onu siler.

   * `CREATE EXTERNAL TABLE`: Hive içinde yeni bir ' External ' tablosu oluşturur. Dış tablolar yalnızca Hive içindeki tablo tanımını depolar (veriler özgün konumda bırakılır).

     > [!NOTE]  
     > Dış tablolar, temel alınan verilerin bir dış kaynak tarafından güncelleştirilmesini beklediğinde kullanılmalıdır. Örneğin, bir MapReduce işi veya Azure hizmeti.
     >
     > Dış tablonun atılması, yalnızca tablo tanımı olan **verileri silmez.**

   * `ROW FORMAT`: Hive verilerin nasıl biçimlendirildiğini söyler. Bu durumda, her günlükteki alanlar boşlukla ayrılır.

   * `STORED AS TEXTFILE LOCATION`: Hive 'ın verilerin örnek/veri dizininde depolandığını ve metin olarak depolandığını söyler.

   * `SELECT`: Sütunun `t4` değeri`[ERROR]`içerdiği tüm satırların sayımını seçin. Bu değer, bu değeri içeren `3` üç satır olduğundan, bir değeri döndürür.

   * `INPUT__FILE__NAME LIKE '%.log'`-Hive 'a yalnızca. log ile biten dosyalardaki verileri döndürmemiz gerektiğini söyler. Bu yan tümce, aramayı verileri içeren Sample. log dosyası ile sınırlandırır.

6. Araç çubuğundan, bu sorgu için kullanmak istediğiniz **HDInsight kümesini** seçin. Deyimlerini Hive işi olarak çalıştırmak için **Gönder** ' i seçin.

   ![Gönderme çubuğu](./media/apache-hadoop-use-hive-visual-studio/hdinsight-toolbar-submit.png)

7. **Hive Iş Özeti** görünür ve çalışan iş hakkında bilgileri görüntüler. İş **durumu** **tamamlanana**kadar değişene kadar Iş bilgilerini yenilemek için **Yenile** bağlantısını kullanın.

   ![tamamlanmış bir işi görüntüleyen iş Özeti](./media/apache-hadoop-use-hive-visual-studio/hdinsight-job-summary.png)

8. Bu işin çıkışını görüntülemek için **Iş çıkışı** bağlantısını kullanın. Bu sorgu `[ERROR] 3`tarafından döndürülen değer olan öğesini görüntüler.

### <a name="additional-example"></a>Ek örnek

Bu örnek, önceki adımda `log4jLogs` oluşturulan tabloya bağımlıdır.

1. **Sunucu Gezgini**, kümenize sağ tıklayın ve **Hive sorgusu yaz**' ı seçin.

2. Aşağıdaki Hive sorgusunu girin:

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Bu deyimler aşağıdaki eylemleri gerçekleştirir:

    * `CREATE TABLE IF NOT EXISTS`: Henüz yoksa tablo oluşturur. `EXTERNAL` Anahtar sözcüğü kullanılmadığından, bu ifade bir iç tablo oluşturur. İç tablolar Hive veri ambarında depolanır ve Hive tarafından yönetilir.
    
    > [!NOTE]  
    > Tabloların `EXTERNAL` aksine, iç tablo bırakılırken temel alınan veriler de silinir.

    * `STORED AS ORC`: Verileri iyileştirilmiş satır sütunlu (ORC) biçimde depolar. ORC, Hive verilerinin depolanması için yüksek düzeyde iyileştirilmiş ve etkili bir biçimdir.
    
    * `INSERT OVERWRITE ... SELECT`: İçeren `log4jLogs` tablodan `errorLogs` satırları seçer, ardından verileri tabloya ekler. `[ERROR]`

3. Sorguyu **Batch** modunda yürütün.

4. İşin tabloyu oluşturduğunu doğrulamak için **Sunucu Gezgini** kullanın ve **Azure** > **HDInsight** 'ı > HDInsight kümeniz > **Hive veritabanlarının** > **varsayılan**genişletin. **Errorlogs** tablosu ve **log4jLogs** tablosu listelenir.

## <a id="nextsteps"></a>Sonraki adımlar

Gördüğünüz gibi, Visual Studio için HDInsight araçları, HDInsight 'ta Hive sorguları ile çalışmanın kolay bir yolunu sunar.

HDInsight 'ta Hive hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive kullanma](hdinsight-use-hive.md)

HDInsight 'ta Hadoop ile birlikte çalışmak için kullanabileceğiniz diğer yollar hakkında daha fazla bilgi için:

* [HDInsight üzerinde Apache Hadoop Apache Pig kullanma](hdinsight-use-pig.md)

* [HDInsight üzerinde Apache Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md)

Visual Studio için HDInsight araçları hakkında daha fazla bilgi için:

* [Visual Studio için HDInsight araçlarını kullanmaya başlama](apache-hadoop-visual-studio-tools-get-started.md)