---
title: Linux tabanlı Azure HDInsight'ta Hadoop Oozie iş akışlarını kullanma
description: Linux tabanlı HDInsight'ta Hadoop Oozie'yi kullanın. Bir Oozie iş akışını nasıl tanımlayıp bir Oozie işi göndereceğini öğrenin.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: ece6fdb743035069bc6c666d6e90c76860f63e82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75744916"
---
# <a name="use-apache-oozie-with-apache-hadoop-to-define-and-run-a-workflow-on-linux-based-azure-hdinsight"></a>Linux tabanlı Azure HDInsight üzerinde iş akışı tanımlamak ve çalıştırmak için Apache Hadoop ile Apache Oozie'yi kullanma

Azure HDInsight'ta Apache Hadoop ile Apache Oozie'yi nasıl kullanacağınızı öğrenin. Oozie Hadoop işleri yöneten bir iş akışı ve koordinasyon sistemidir. Oozie Hadoop yığını ile entegre edilmiştir ve aşağıdaki işleri destekler:

* Apache Hadoop HaritasıAzalt
* Apaçi Domuz
* Apaçi Kovanı
* Apaçi Sqoop

Oozie'yi, Java programları veya kabuk komut dosyaları gibi bir sisteme özgü işleri zamanlamak için de kullanabilirsiniz.

> [!NOTE]  
> HDInsight ile iş akışlarını tanımlamak için başka bir seçenek azure veri fabrikası nı kullanmaktır. Veri Fabrikası hakkında daha fazla bilgi edinmek için Veri [Fabrikası ile Apache Pig ve Apache Hive kullanın'a][azure-data-factory-pig-hive]bakın. Kurumsal Güvenlik Paketi ile kümeler üzerinde Oozie kullanmak için [Kurumsal Güvenlik Paketi ile HDInsight Hadoop kümeleri Çalıştır Apache Oozie](domain-joined/hdinsight-use-oozie-domain-joined-clusters.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* **HDInsight'ta hadoop kümesi.** [Linux'ta HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)bakın.

* **Bir SSH istemcisi**. [Bkz. SSH kullanarak HDInsight'a (Apache Hadoop) bağlanın.](hdinsight-hadoop-linux-use-ssh-unix.md)

* **Azure SQL Veritabanı.**  Bkz. [Azure portalında bir Azure SQL veritabanı oluşturun.](../sql-database/sql-database-get-started.md)  Bu **makalede, oozietest**adlı bir veritabanı kullanır.

* Kümeleriniz birincil depolama için [URI düzeni.](./hdinsight-hadoop-linux-information.md#URI-and-scheme) Bu, `wasb://` Azure Depolama, `abfs://` Azure Veri Gölü Depolama `adl://` Gen2 veya Azure Veri Gölü Depolama Gen1 için olacaktır. Azure Depolama için güvenli aktarım etkinleştirilirse, URI `wasbs://`. Ayrıca bakınız, [güvenli aktarım.](../storage/common/storage-require-secure-transfer.md)

## <a name="example-workflow"></a>Örnek iş akışı

Bu belgede kullanılan iş akışı iki eylem içerir. Eylemler, Kovan, Sqoop, MapReduce veya diğer işlemleri çalıştırma gibi görevleriçin tanımlardır:

![HDInsight oozie iş akışı diyagramı](./media/hdinsight-use-oozie-linux-mac/oozie-workflow-diagram.png)

1. Bir Hive eylem HDInsight ile birlikte kayıtları `hivesampletable` ayıklamak için bir HiveQL komut dosyası çalışır. Her veri satırı, belirli bir mobil aygıtın ziyaretini açıklar. Kayıt biçimi aşağıdaki metin gibi görünür:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Bu belgede kullanılan Hive komut dosyası, Android veya iPhone gibi her platform için yapılan toplam ziyaretleri sayar ve sayımları yeni bir Hive tablosuna depolar.

    Hive hakkında daha fazla bilgi için [HDInsight ile Apache Hive kullanın'a][hdinsight-use-hive]bakın.

2. Bir Sqoop eylemi, yeni Hive tablosunun içeriğini Azure SQL Veritabanı'nda oluşturulan bir tabloya dışa yükler. Sqoop hakkında daha fazla bilgi için, [HDInsight ile Apache Sqoop kullanın][hdinsight-use-sqoop]bakın.

> [!NOTE]  
> HDInsight kümelerinde desteklenen Oozie sürümleri [için, HDInsight tarafından sağlanan Hadoop küme sürümlerinde yeniliklere][hdinsight-versions]bakın.

## <a name="create-the-working-directory"></a>Çalışma dizini oluşturma

Oozie, bir iş için gereken tüm kaynakları aynı dizinde depolamanızı bekler. Bu örnekte . `wasbs:///tutorials/useoozie` Bu dizini oluşturmak için aşağıdaki adımları tamamlayın:

1. Kümenin SSH kullanıcı `sshuser` adı ile değiştirmek için aşağıdaki kodu `CLUSTERNAME` ve kümenin adıyla değiştirin.  Daha sonra [SSH kullanarak](hdinsight-hadoop-linux-use-ssh-unix.md)HDInsight kümesine bağlanmak için kodu girin.  

    ```bash
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Dizin oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -mkdir -p /tutorials/useoozie/data
    ```

    > [!NOTE]  
    > Parametre, `-p` yoldaki tüm dizinlerin oluşturulmasına neden olur. `useooziewf.hql` Dizin, `data` komut dosyası tarafından kullanılan verileri tutmak için kullanılır.

3. SSH kullanıcı adınız `sshuser` ile değiştirmek için aşağıdaki kodu edin.  Oozie'nin kullanıcı hesabınızı taklit edebileceğinden emin olmak için aşağıdaki komutu kullanın:

    ```bash
    sudo adduser sshuser users
    ```

    > [!NOTE]  
    > Kullanıcının zaten `users` grubun bir üyesi olduğunu gösteren hataları yoksayabilirsiniz.

## <a name="add-a-database-driver"></a>Veritabanı sürücüsü ekleme

Bu iş akışı, verileri SQL veritabanına aktarmak için Sqoop kullandığından, SQL veritabanıyla etkileşimde bulunan JDBC sürücüsünün bir kopyasını sağlamanız gerekir. JDBC sürücüsünü çalışma dizinine kopyalamak için SSH oturumundaki aşağıdaki komutu kullanın:

```bash
hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc*.jar /tutorials/useoozie/
```

> [!IMPORTANT]  
> 'de `/usr/share/java/`bulunan gerçek JDBC sürücüsünü doğrulayın

İş akışınız MapReduce uygulaması içeren bir kavanoz gibi başka kaynaklar kullandıysa, bu kaynakları da eklemeniz gerekir.

## <a name="define-the-hive-query"></a>Kovan sorgusunu tanımlama

Bir sorgu tanımlayan bir Hive sorgu dili (HiveQL) komut dosyası oluşturmak için aşağıdaki adımları kullanın. Sorguyu daha sonra bu belgede Bir Oozie iş akışında kullanırsınız.

1. SSH bağlantısından, aşağıdaki komutu kullanarak aşağıdaki `useooziewf.hql`leri kullanarak aşağıdaki leri kullanarak aşağıdaki leri kullanarak:

    ```bash
    nano useooziewf.hql
    ```

1. GNU nano düzenleyicisi açıldıktan sonra, dosyanın içeriği olarak aşağıdaki sorguyu kullanın:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Komut dosyasında kullanılan iki değişken vardır:

   * `${hiveTableName}`: Oluşturulacak tablonun adını içerir.

   * `${hiveDataFolder}`: Tablonun veri dosyalarını depolamak için gereken konumu içerir.

     İş akışı tanımı dosyası, bu makalede iş akışı.xml, çalışma zamanında bu HiveQL komut dosyasına bu değerleri geçer.

1. Dosyayı kaydetmek için **Ctrl+X'i**seçin, **Y**girin ve sonra **Enter'u**seçin.  

1. Kopyalamak için `useooziewf.hql` aşağıdaki `wasbs:///tutorials/useoozie/useooziewf.hql`komutu kullanın:

    ```bash
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Bu komut, `useooziewf.hql` dosyayı küme için HDFS uyumlu depolama alanında saklar.

## <a name="define-the-workflow"></a>İş akışını tanımlama

Oozie iş akışı tanımları, XML işlem tanımı dili olan Hadoop Process Definition Language (hPDL) ile yazılır. İş akışını tanımlamak için aşağıdaki adımları kullanın:

1. Yeni bir dosya oluşturmak ve bunları yeniden oluşturmak için aşağıdaki deyimi kullanın:

    ```bash
    nano workflow.xml
    ```

2. Nano düzenleyici açıldıktan sonra, dosya içeriği olarak aşağıdaki XML'i girin:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>
        <action name="RunHiveScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScript}</script>
            <param>hiveTableName=${hiveTableName}</param>
            <param>hiveDataFolder=${hiveDataFolder}</param>
        </hive>
        <ok to="RunSqoopExport"/>
        <error to="fail"/>
        </action>
        <action name="RunSqoopExport">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.compress.map.output</name>
                <value>true</value>
            </property>
            </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveDataFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
            </sqoop>
        <ok to="end"/>
        <error to="fail"/>
        </action>
        <kill name="fail">
        <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>
        <end name="end"/>
    </workflow-app>
    ```

    İş akışında tanımlanan iki eylem vardır:

   * `RunHiveScript`: Bu eylem başlangıç eylemidir `useooziewf.hql` ve Hive komut dosyasını çalıştırın.

   * `RunSqoopExport`: Bu eylem, Sqoop kullanarak Hive komut dosyasından oluşturulan verileri SQL veritabanına aktarıyor. Bu eylem yalnızca `RunHiveScript` eylem başarılı olursa çalışır.

     İş akışı gibi `${jobTracker}`birkaç girişi vardır. Bu girişleri iş tanımında kullandığınız değerlerle değiştirirsiniz. İş tanımını daha sonra bu belgede oluşturursunuz.

     Ayrıca Sqoop bölümünde `<archive>mssql-jdbc-7.0.0.jre8.jar</archive>` giriş unutmayın. Bu giriş, Oozie'ye bu işlem çalıştığında bu arşivi Sqoop için kullanılabilir hale getirmesini emreder.

3. Dosyayı kaydetmek için **Ctrl+X'i**seçin, **Y**girin ve sonra **Enter'u**seçin.  

4. Dosyayı kopyalamak için `workflow.xml` `/tutorials/useoozie/workflow.xml`aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-a-table"></a>Bir tablo oluşturma

> [!NOTE]  
> Tablo oluşturmak için SQL Veritabanı'na bağlanmanın birçok yolu vardır. Aşağıdaki adımlarda HDInsight kümesinden [FreeTDS](https://www.freetds.org/) kullanılır.

1. HDInsight kümesine FreeTDS yüklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Azure SQL sunucu adınız ve `<serverName>` `<sqlLogin>` Azure SQL sunucu girişi ile değiştirmek için aşağıdaki kodu düzenleme.  Ön koşuldaki SQL veritabanına bağlanmak için komutu girin.  Parolayı istek te girin.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -p 1433 -D oozietest
    ```

    Aşağıdaki metin gibi çıktı alırsınız:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

3. `1>` isteminde aşağıdaki satırları girin:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    `GO` deyimi girildiğinde önceki deyimler değerlendirilir. Bu ifadeler, iş `mobiledata`akışı tarafından kullanılan , adlı bir tablo oluşturur.

    Tablonun oluşturulduğunu doğrulamak için aşağıdaki komutları kullanın:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Çıktıyı aşağıdaki metin gibi görürsünüz:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo             mobiledata      BASE TABLE

4. İstem de girerek `exit` tsql yardımcı programı çıkın. `1>`

## <a name="create-the-job-definition"></a>İş tanımını oluşturma

İş tanımı iş akışı.xml nerede bulunacağını açıklar. Ayrıca, iş akışı tarafından kullanılan diğer dosyaların nerede `useooziewf.hql`bulunacağını da açıklar. Ayrıca, iş akışı ve ilişkili dosyalar içinde kullanılan özellikleri için değerleri tanımlar.

1. Varsayılan depolama alanının tam adresini almak için aşağıdaki komutu kullanın. Bu adres, bir sonraki adımda oluşturduğunuz yapılandırma dosyasında kullanılır.

    ```bash
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Bu komut aşağıdaki XML gibi bilgileri döndürür:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]  
    > HDInsight kümesi varsayılan depolama alanı olarak Azure `<value>` Depolama'yı `wasbs://`kullanıyorsa, öğe içeriği . Bunun yerine Azure Veri Gölü Depolama Gen1 `adl://`kullanılırsa, 'ile başlar. Azure Veri Gölü Depolama Gen2 kullanılırsa, 'ile `abfs://`başlar.

    Sonraki adımlarda `<value>` kullanıldığı gibi öğenin içeriğini kaydedin.

2. Aşağıdaki xml'i aşağıdaki gibi edin:

    |Yer tutucu değeri| Değiştirilen değer|
    |---|---|
    |wasbs://mycontainer\@mystorageaccount.blob.core.windows.net| Adım 1'den alınan değer.|
    |yönetici| Admin değilse HDInsight kümesi için giriş adınız.|
    |Sunucuadı| Azure SQL veritabanı sunucu adı.|
    |sqlLogin| Azure SQL veritabanı sunucusu girişi.|
    |sqlPassword| Azure SQL veritabanı sunucu giriş parolası.|

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
        </property>

        <property>
        <name>jobTracker</name>
        <value>headnodehost:8050</value>
        </property>

        <property>
        <name>queueName</name>
        <value>default</value>
        </property>

        <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
        </property>

        <property>
        <name>hiveScript</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=sqlLogin;password=sqlPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>admin</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

    Bu dosyadaki bilgilerin çoğu iş akışı.xml veya ooziewf.hql dosyaları, gibi `${nameNode}`kullanılan değerleri doldurmak için kullanılır.  Yol bir `wasbs` yolsa, tam yolu kullanmalısınız. Sadece `wasbs:///`kısaltmak yok . Giriş, `oozie.wf.application.path` iş akışı.xml dosyasını nerede bulacağını tanımlar. Bu dosya, bu iş tarafından çalıştırılabilen iş akışını içerir.

3. Oozie iş tanımı yapılandırmasını oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano job.xml
    ```

4. Nano düzenleyici açıldıktan sonra, düzenlenen XML'yi dosyanın içeriği olarak yapıştırın.

5. Dosyayı kaydetmek için **Ctrl+X'i**seçin, **Y**girin ve sonra **Enter'u**seçin.

## <a name="submit-and-manage-the-job"></a>İşi gönderme ve yönetme

Aşağıdaki adımlar kümedeki Oozie iş akışlarını göndermek ve yönetmek için Oozie komutunu kullanır. Oozie komutu [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html)üzerinde dostane bir arayüzdür.

> [!IMPORTANT]  
> Oozie komutunu kullandığınızda, HDInsight kafa düğümü için FQDN'yi kullanmanız gerekir. Bu FQDN'ye yalnızca kümeden veya küme bir Azure sanal ağındaysa, aynı ağdaki diğer makinelerden erişilebilir.

1. Oozie hizmetinin URL'sini elde etmek için aşağıdaki komutu kullanın:

    ```bash
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Bu, aşağıdaki XML gibi bilgileri döndürür:

    ```xml
    <name>oozie.base.url</name>
    <value>http://ACTIVE-HEADNODE-NAME.UNIQUEID.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    Bölüm, `http://ACTIVE-HEADNODE-NAME.UNIQUEID.cx.internal.cloudapp.net:11000/oozie` Oozie komutuyla kullanılacak URL'dir.

2. URL'yi daha önce aldığınız url ile değiştirmek için kodu edin. URL için bir ortam değişkeni oluşturmak için, her komut için girmeniz gerekmesin diye aşağıdakileri kullanın:

    ```bash
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

3. İşi göndermek için aşağıdakileri kullanın:

    ```bash
    oozie job -config job.xml -submit
    ```

    Bu komut iş bilgilerini `job.xml` yükler ve Oozie'ye gönderir, ancak çalıştırmaz.

    Komut bittikten sonra, örneğin işin kimliğini `0000005-150622124850154-oozie-oozi-W`döndürmelidir. Bu kimlik, işi yönetmek için kullanılır.

4. Önceki adımda döndürülen `<JOBID>` kimlikle değiştirmek için aşağıdaki kodu edin.  İşin durumunu görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    oozie job -info <JOBID>
    ```

    Bu, aşağıdaki metin gibi bilgileri döndürür:

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasb:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    Bu iş bir `PREP`durumu vardır . Bu durum, işin oluşturulduğunu, ancak başlatılmadığını gösterir.

5. Daha önce döndürülen `<JOBID>` kimlikle değiştirmek için aşağıdaki kodu edin.  İşi başlatmak için aşağıdaki komutu kullanın:

    ```bash
    oozie job -start <JOBID>
    ```

    Bu komuttan sonra durumu denetlerseniz, çalışan bir durumdadır ve iş içindeki eylemler için bilgiler döndürülür.  İşi tamamlaması birkaç dakika sürer.

6. Azure SQL sunucu adınız ve `<serverName>` `<sqlLogin>` Azure SQL sunucu girişi ile değiştirmek için aşağıdaki kodu düzenleme.  Görev başarıyla *tamamlandıktan sonra,* aşağıdaki komutu kullanarak verilerin oluşturulduğunu ve SQL veritabanı tablosuna dışa aktarıldığını doğrulayabilirsiniz.  Parolayı istek te girin.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -p 1433 -D oozietest
    ```

    İsteyerek, `1>` aşağıdaki sorguyu girin:

    ```sql
    SELECT * FROM mobiledata
    GO
    ```

    Döndürülen bilgiler aşağıdaki metin gibidir:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Oozie komutu hakkında daha fazla bilgi için [Apache Oozie komut satırı aracına](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html)bakın.

## <a name="oozie-rest-api"></a>Oozie REST API

Oozie REST API ile, Oozie ile çalışan kendi araçları nızı oluşturabilirsiniz. Aşağıda, Oozie REST API'nin kullanımı hakkında HDInsight'a özgü bilgiler vereme

* **URI**: REST API'ye `https://CLUSTERNAME.azurehdinsight.net/oozie`kümenin dışından .

* Kimlik doğrulama : Kimlik **doğrulaması**için API'yi http hesabı (yönetici) ve parola kümesini kullanın. Örnek:

    ```bash
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

Oozie REST API'nin nasıl kullanılacağı hakkında daha fazla bilgi için [Apache Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html)adresine bakın.

## <a name="oozie-web-ui"></a>Oozie web UI

Oozie web Kullanıcı İyiuBilgisi kümedeki Oozie işlerinin durumuna web tabanlı bir görünüm sağlar. Web Kullanıcı Arama Hizmeti ile aşağıdaki bilgileri görüntüleyebilirsiniz:

   * İş durumu
   * İş tanımı
   * Yapılandırma
   * İşteki eylemlerin grafiği
   * İş için günlükler

Ayrıca, bir iş içindeki eylemlerin ayrıntılarını da görüntüleyebilirsiniz.

Oozie web UI'sine erişmek için aşağıdaki adımları tamamlayın:

1. HDInsight kümesine bir SSH tüneli oluşturun. Daha fazla bilgi için [hdinsight ile SSH Tünel Kullanma'ya](hdinsight-linux-ambari-ssh-tunnel.md)bakın.

2. Bir tünel oluşturduktan sonra, URI `http://headnodehost:8080`kullanarak web tarayıcınızda Ambari web UI'yi açın.

3. Sayfanın sol tarafından, **Oozie** > **Hızlı Bağlantılar** > **Oozie Web UI**seçin.

    ![Apache Ambari oozie web ui adımları](./media/hdinsight-use-oozie-linux-mac/hdi-oozie-web-ui-steps.png)

4. Oozie web UI, çalışan iş akışı işlerini görüntülemek için varsayılan dır. Tüm iş akışı işlerini görmek için **Tüm İşler'i**seçin.

    ![Oozie web konsolu iş akışı işleri](./media/hdinsight-use-oozie-linux-mac/hdinsight-oozie-jobs.png)

5. Bir iş hakkında daha fazla bilgi görüntülemek için işi seçin.

    ![HDInsight Apache Oozie iş bilgileri](./media/hdinsight-use-oozie-linux-mac/hdinsight-oozie-job-info.png)

6. İş **Bilgileri** sekmesinden, temel iş bilgilerini ve iş içindeki tek tek eylemleri görebilirsiniz. **İş Tanımı, İş** **Yapılandırması,** **İş Günlüğü'ne**erişmek veya **Job DAG**altında işin yönlendirilmiş bir asiklik grafiğini (DAG) görüntülemek için üstteki sekmeleri kullanabilirsiniz.

   * **İş Günlüğü**: İş için tüm günlükleri almak için **Günlükleri Al** düğmesini seçin veya günlükleri filtrelemek için **Arama Filtresini Gir** alanını kullanın.

       ![HDInsight Apache Oozie iş günlüğü](./media/hdinsight-use-oozie-linux-mac/hdinsight-oozie-job-log.png)

   * **İş DAG**: DAG, iş akışı üzerinden alınan veri yollarının grafiksel bir özetidir.

       ![HDInsight Apache Oozie iş dag](./media/hdinsight-use-oozie-linux-mac/hdinsight-oozie-job-dag.png)

7. **İş Bilgileri** sekmesinden eylemlerden birini seçerseniz, eylem için bilgi getirir. Örneğin, **RunSqoopExport** eylemini seçin.

    ![HDInsight oozie iş eylem bilgi](./media/hdinsight-use-oozie-linux-mac/oozie-job-action-info.png)

8. **Konsol URL**bağlantısı gibi eylemin ayrıntılarını görebilirsiniz. İş için iş izleyici bilgilerini görüntülemek için bu bağlantıyı kullanın.

## <a name="schedule-jobs"></a>İşleri zamanlama

İş başlangıcı, sonu ve iş oluşumu sıklığını belirtmek için koordinatörü kullanabilirsiniz. İş akışı için bir zamanlama tanımlamak için aşağıdaki adımları tamamlayın:

1. **coordinator.xml**adlı bir dosya oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano coordinator.xml
    ```

    Dosyanın içeriği olarak aşağıdaki XML'yi kullanın:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
        <workflow>
            <app-path>${workflowPath}</app-path>
        </workflow>
        </action>
    </coordinator-app>
    ```

    > [!NOTE]  
    > `${...}` Değişkenler çalışma zamanında iş tanımındaki değerlerle değiştirilir. Değişkenler şunlardır:
    >
    > * `${coordFrequency}`: İşin çalışan örnekleri arasındaki süre.
    > * `${coordStart}`: İş başlangıç saati.
    > * `${coordEnd}`: İş bitiş saati.
    > * `${coordTimezone}`: Koordinatör işler, genellikle UTC kullanılarak temsil edilen, gün ışığından yararlanma saati olmayan sabit bir saat dilimindedir. Bu saat *dilimi, Oozie işleme saat dilimi* olarak adlandırılır.
    > * `${wfPath}`: İş akışı.xml yolu.

2. Dosyayı kaydetmek için **Ctrl+X'i**seçin, **Y**girin ve sonra **Enter'u**seçin.

3. Dosyayı bu iş için çalışma dizinine kopyalamak için aşağıdaki komutu kullanın:

    ```bash
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. Daha önce `job.xml` oluşturduğunuz dosyayı değiştirmek için aşağıdaki komutu kullanın:

    ```bash
    nano job.xml
    ```

    Aşağıdaki değişiklikleri yapın:

   * Oozie'ye iş akışı yerine koordinatör dosyasını `<name>oozie.wf.application.path</name>` çalıştırmasını söylemek için ' ' e `<name>oozie.coord.application.path</name>`değiştirin

   * Koordinatör tarafından `workflowPath` kullanılan değişkeni ayarlamak için aşağıdaki XML'yi ekleyin:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       `wasbs://mycontainer@mystorageaccount.blob.core.windows` Metni job.xml dosyasındaki diğer girişlerde kullanılan değerle değiştirin.

   * Koordinatörün başlangıç, bitiş ve sıklığını tanımlamak için aşağıdaki XML'yi ekleyin:

        ```xml
        <property>
            <name>coordStart</name>
            <value>2018-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2018-05-12T12:00Z</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>1440</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>UTC</value>
        </property>
        ```

       Bu değerler başlangıç saatini 10 Mayıs 2018 tarihinde saat 12:00'ye, bitiş saatini ise 12 Mayıs 2018'e ayarla. Bu işi çalıştırma aralığı günlük olarak ayarlanır. Frekans dakika, bu nedenle 24 saat x 60 dakika = 1440 dakika. Son olarak, saat dilimi UTC olarak ayarlanır.

5. Dosyayı kaydetmek için **Ctrl+X'i**seçin, **Y**girin ve sonra **Enter'u**seçin.

6. İşi göndermek ve başlatmak için aşağıdaki komutu kullanın:

    ```bash
    oozie job -config job.xml -run
    ```

7. Oozie web Kullanıcı Arama Hizmeti'ne gider ve **Koordinatör İşler** sekmesini seçerseniz, aşağıdaki resimdeki gibi bilgileri görürsünüz:

    ![Oozie web konsolu koordinatörü işleri sekmesi](./media/hdinsight-use-oozie-linux-mac/coordinator-jobs-tab.png)

    **Sonraki Maddeleştirme** girişi, işin bir sonraki çalışmasının bir sonraki zamanını içerir.

8. Önceki iş akışı işi gibi, web Kullanıcı Arama Hizmeti'ndeki iş girişini seçerseniz, iş hakkındaki bilgileri görüntüler:

    ![Apache Oozie koordinatör iş bilgileri](./media/hdinsight-use-oozie-linux-mac/coordinator-job-info.png)

    > [!NOTE]  
    > Bu görüntü, zamanlanan iş akışı içindeki tek tek eylemleri değil, yalnızca işin başarılı çalışanlarını gösterir. Tek tek eylemleri görmek için **Eylem** girişlerinden birini seçin.

    ![OOzie web konsolu iş bilgileri sekmesi](./media/hdinsight-use-oozie-linux-mac/coordinator-action-job.png)

## <a name="troubleshooting"></a>Sorun giderme

Oozie UI ile Oozie günlüklerini görüntüleyebilirsiniz. Oozie UI ayrıca iş akışı tarafından başlatılan MapReduce görevleri için JobTracker günlüklerine bağlantılar içerir. Sorun giderme deseni şöyle olmalıdır:

   1. Oozie web UI'deki işi görüntüleyin.

   2. Belirli bir eylem için bir hata veya hata varsa, **Hata İletisi** alanının hata hakkında daha fazla bilgi sağp sağlamadığı görmek için eylemi seçin.

   3. Varsa, eylem için JobTracker günlükleri gibi daha fazla ayrıntıyı görüntülemek için eylemdeki URL'yi kullanın.

Karşılaşabileceğiniz belirli hatalar ve bunları nasıl çözeceğiniz aşağıda verilmiştir.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: Kümeyi başharfleyemez

**Belirtileri**: İş durumu **SUSPENDED'e**dönüşür. İş ayrıntıları **START_MANUAL** `RunHiveScript` olarak durumunu gösterir. Eylemi seçmek aşağıdaki hata iletisini görüntüler:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Neden**: **job.xml** dosyasında kullanılan Azure Blob depolama adresleri depolama kapsayıcısı veya depolama hesabı adı içermez. Blob depolama adresi biçimi `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Çözüm**: İşin kullandığı Blob depolama adreslerini değiştirin.

### <a name="ja002-oozie-is-not-allowed-to-impersonate-ltusergt"></a>JA002: Oozie KULLANICI taklit &lt;etmek için izin verilmez&gt;

**Belirtileri**: İş durumu **SUSPENDED'e**dönüşür. İş ayrıntıları **START_MANUAL** `RunHiveScript` olarak durumunu gösterir. Eylemi seçerseniz, aşağıdaki hata iletisini gösterir:

    JA002: User: oozie is not allowed to impersonate <USER>

**Neden**: Geçerli izin ayarları, Oozie'nin belirtilen kullanıcı hesabını taklit etmesine izin vermez.

**Çözünürlük**: Oozie, **kullanıcı** grubundaki kullanıcıları taklit edebilir. Kullanıcı `groups USERNAME` hesabının üyesi olduğu grupları görmek için kullanın. Kullanıcı **kullanıcı** grubunun bir üyesi değilse, kullanıcıyı gruba eklemek için aşağıdaki komutu kullanın:

    sudo adduser USERNAME users

> [!NOTE]  
> HDInsight'ın kullanıcının gruba eklendiğini fark etmesi birkaç dakika sürebilir.

### <a name="launcher-error-sqoop"></a>Başlatıcı HATASI (Sqoop)

**Belirtileri**: İş durumu **KILLED**olarak değişir. İş ayrıntıları **HATA** `RunSqoopExport` olarak durumu gösterir. Eylemi seçerseniz, aşağıdaki hata iletisini gösterir:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Neden**: Sqoop veritabanına erişmek için gereken veritabanı sürücüsünü yükleyemiyor.

**Çözünürlük**: Bir Oozie işinden Sqoop kullandığınızda, iş akışı.xml gibi diğer kaynaklara veritabanı sürücüsü eklemeniz gerekir, iş kullanır. Ayrıca, iş akışı.xml `<sqoop>...</sqoop>` bölümünden veritabanı sürücüsü içeren arşiv başvuru.

Örneğin, bu belgedeki iş için aşağıdaki adımları kullanırsınız:

1. Dosyayı `mssql-jdbc-7.0.0.jre8.jar` **/tutorials/useoozie** dizinine kopyalayın:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre8.jar /tutorials/useoozie/mssql-jdbc-7.0.0.jre8.jar
    ```

2. Yukarıdaki `</sqoop>` `workflow.xml` yeni bir satıra aşağıdaki XML'yi eklemek için değiştirin:

    ```xml
    <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Oozie iş akışını nasıl tanımlayabileceğinizi ve bir Oozie işini nasıl çalıştırabileceğinizi öğrendiniz. HDInsight ile nasıl çalışacağınız hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [HDInsight'ta Apache Hadoop işleri için veri yükleyin][hdinsight-upload-data]
* [HDInsight'ta Apache Hadoop ile Apache Sqoop kullanın][hdinsight-use-sqoop]
* [HDInsight'ta Apache Hadoop ile Apache Hive kullanın][hdinsight-use-hive]
* [HDInsight için Java MapReduce programları geliştirin](hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md)

[azure-data-factory-pig-hive]: ../data-factory/transform-data.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-use-sqoop]:hadoop/apache-hadoop-use-sqoop-mac-linux.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
