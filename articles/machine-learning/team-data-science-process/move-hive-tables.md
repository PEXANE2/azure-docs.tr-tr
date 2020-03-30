---
title: Blob depolamadan kovan tabloları oluşturun ve veri yükleyin - Ekip Veri Bilimi Süreci
description: Hive tabloları oluşturmak ve Azure blob depolamadan veri yüklemek için Kovan sorgularını kullanın. Partition Hive tabloları ve sorgu performansını artırmak için Optimize Edilmiş Satır Sütun (ORC) biçimlendirmesini kullanın.
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
ms.openlocfilehash: 625d9d5c5ecf095d4acbff625754b2065f184536
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251667"
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Azure Blob Depolama'dan Kovan tabloları oluşturun ve veri yükleyin

Bu makalede, Hive tabloları ve Azure blob depolama dan veri yüklemek oluşturmak genel Hive sorguları sunar. Hive tablolarını bölümleme ve sorgu performansını artırmak için Optimize Edilmiş Satır Sütunu (ORC) biçimlendirmesini kullanma konusunda da bazı kılavuzlar sağlanır.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sahip olduğunuzu varsayar:

* Bir Azure Depolama hesabı oluşturdu. Yönergelere ihtiyacınız varsa, [Azure Depolama hesapları hakkında](../../storage/common/storage-introduction.md)bakın.
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi sağlanmış.  Talimatlara ihtiyacınız varsa, [HDInsight'ta Kurulum Kümeleri'ne](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)bakın.
* Kümeye uzaktan erişimi sağladı, oturum açtı ve Hadoop Command-Line konsolunu açtı. Talimatlara ihtiyacınız varsa, [bkz.](../../hdinsight/hdinsight-administer-use-portal-linux.md)

## <a name="upload-data-to-azure-blob-storage"></a>Azure blob depolamasına veri yükleme
[Gelişmiş analitik için Bir Azure sanal makine ayarla'da](../../machine-learning/data-science-virtual-machine/overview.md)sağlanan yönergeleri izleyerek bir Azure sanal makinesi oluşturduysanız, bu komut dosyası dosyası sanal makinedeki *C:\\\\\<Users kullanıcı\>\\adı Belgeler\\Veri Bilimi Komut Dizini'ne* indirilmiş olmalıdır. Bu Kovan sorguları yalnızca gönderim için hazır olmak için uygun alanlarda bir veri şeması ve Azure blob depolama yapılandırması sağlamanızı gerektirir.

Hive tablolarıiçin verilerin **sıkıştırılmamış** bir tablo biçiminde olduğunu ve verilerin Hadoop kümesi tarafından kullanılan depolama hesabının varsayılan (veya ek) kapsayıcısına yüklendiğini varsayıyoruz.

**EĞER NYC Taksi Trip Veri**üzerinde uygulama yapmak istiyorsanız, yapmanız gerekir:

* 24 [NYC Taksi Gezisi Veri](https://www.andresmh.com/nyctaxitrips) dosyaları (12 Trip dosyaları ve 12 Ücret dosyaları) **indirin,**
* .csv dosyalarına tüm dosyaları **unzip** ve sonra
* bunları Azure Depolama hesabının varsayılan (veya uygun kapsayıcısına) **yükleyin;** böyle bir hesabın seçenekleri [Azure HDInsight kümeleri konusuyla Azure Depolama'yı kullan'da](../../hdinsight/hdinsight-hadoop-use-blob-storage.md) görünür. .csv dosyalarını depolama hesabındaki varsayılan kapsayıcıya yükleme işlemi bu [sayfada](hive-walkthrough.md#upload)bulunabilir.

## <a name="how-to-submit-hive-queries"></a><a name="submit"></a>Hive sorguları nasıl gönderilir?
Kovan sorguları kullanılarak gönderilebilir:

* [Hadoop kümesinin headnode Hadoop Komut Hattı üzerinden Hive sorguları gönderin](#headnode)
* [Hive Düzenleyicisi ile Hive sorguları gönder](#hive-editor)
* [Azure PowerShell Komutları ile Kovan sorguları gönderme](#ps)

Kovan sorguları SQL benzeridir. SQL'e aşinaysanız, [SQL Users Cheat Sheet için Hive'ı](https://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) yararlı bulabilirsiniz.

Bir Hive sorgusu gönderirken, ister ekranda ister kafa düğümündeki yerel bir dosyada veya bir Azure blob'unda olsun, Hive sorgularından çıktının hedefini de denetleyebilirsiniz.

### <a name="submit-hive-queries-through-hadoop-command-line-in-headnode-of-hadoop-cluster"></a><a name="headnode"></a>Hadoop kümesinin headnode Hadoop Komut Hattı üzerinden Hive sorguları gönderin
Hive sorgusu karmaşıksa, doğrudan Hadoop kümesinin baş düğümüne göndermek genellikle bir Hive Düzenleyicisi veya Azure PowerShell komut dosyalarıyla göndermekten daha hızlı dönmeye yol açar.

Hadoop kümesinin baş düğümüne giriş yapın, baş düğümünün masaüstünde Hadoop Komut Hattı'nı açın ve komutu `cd %hive_home%\bin`girin.

Hiop Komut Satırı'nda Hive sorgularını göndermenin üç yolu vardır:

* Doğru -dan
* '.hql' dosyalarını kullanma
* Hive komut konsolu ile

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Hive sorgularını doğrudan Hadoop Komuta Hattı'na gönderin.
Basit Hive `hive -e "<your hive query>;` sorgularını doğrudan Hadoop Komut Hattı'na göndermek gibi komutu çalıştırabilirsiniz. Burada, kırmızı kutunun Hive sorgusunu gönderen komutu ve yeşil kutunun Hive sorgusundaki çıktıyı özetlediği bir örnek verilmiştir.

![Hive sorgusundan çıktı ile Hive sorgusu göndermek için komut](./media/move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>'.hql' dosyalarında Kovan sorguları gönderme
Hive sorgusu daha karmaşık olduğunda ve birden çok satırı olduğunda, komut satırında veya Hive komut konsolunda sorguları düzenleme pratik değildir. Alternatif, Baş düğümünün yerel dizindeki '.hql' dosyasındaki Hive sorgularını kaydetmek için Hadoop kümesinin baş düğümünde bir metin düzenleyicisi kullanmaktır. Daha sonra '.hql' dosyasındaki Hive sorgusu aşağıdaki `-f` gibi bağımsız değişken kullanılarak gönderilebilir:

    hive -f "<path to the '.hql' file>"

!['.hql' dosyasında kovan sorgusu](./media/move-hive-tables/run-hive-queries-3.png)

**Hive sorgularının ilerleme durumunu ekran yazdırmayı bastırma**

Varsayılan olarak, Hive sorgusu Hadoop Komut Satırı'na gönderildikten sonra, Harita/Azalt işinin ilerlemesi ekrana yazdırılır. Harita/Azalt iş ilerlemesinin ekran baskısını bastırmak için `-S` komut satırında aşağıdaki gibi bir bağımsız değişken ("Büyük harfli S" kullanabilirsiniz:

    hive -S -f "<path to the '.hql' file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Hive komut konsolunda Kovan sorguları gönderin.
Ayrıca önce Hadoop Komut Hattı'nda `hive` komut çalıştırarak Hive komut konsoluna girebilir ve ardından Kovan komut konsoluna Hive sorguları gönderebilirsiniz. Aşağıda bir örnek vardır. Bu örnekte, iki kırmızı kutu Hive komut konsolunu girmek için kullanılan komutları ve sırasıyla Hive komut konsolunda gönderilen Hive sorgusunu vurgular. Yeşil kutu, Kovan sorgusundaki çıktıyı vurgular.

![Kovan komut konsolunu açın ve komutu girin, Kovan sorgu çıktısını görüntüle](./media/move-hive-tables/run-hive-queries-2.png)

Önceki örnekler doğrudan ekranda Hive sorgu sonuçları çıktı. Çıktıyı baş düğümündeki yerel bir dosyaya veya Azure blob'una da yazabilirsiniz. Ardından, Hive sorgularının çıktısını daha fazla çözümlemek için diğer araçları kullanabilirsiniz.

**Çıkış Kovan sorgu sonuçları yerel bir dosyaya.**
Hive sorgu sonuçlarını baş düğümündeki yerel bir dizine çıktı olarak, Hadoop Komut Satırındaki Hive sorgusunu aşağıdaki gibi göndermeniz gerekir:

    hive -e "<hive query>" > <local path in the head node>

Aşağıdaki örnekte, Kovan sorgusunun çıktısı dizindeki `hivequeryoutput.txt` `C:\apps\temp`bir dosyaya yazılır.

![Kovan sorgusunun çıktısı](./media/move-hive-tables/output-hive-results-1.png)

**Azure blob'una Çıkış Kovanı sorgusu sonuçları**

Hive sorgu sonuçlarını Hadoop kümesinin varsayılan kapsayıcısı içinde bir Azure blob'una da çıktılayabilirsiniz. Bunun için Hive sorgusu aşağıdaki gibidir:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Aşağıdaki örnekte, Hive sorgusunun çıktısı Hadoop `queryoutputdir` kümesinin varsayılan kapsayıcısı içindeki bir blob dizinine yazılır. Burada, yalnızca blob adı olmadan dizin adını sağlamanız gerekir. Hem dizin hem de blob adları sağlarsanız `wasb:///queryoutputdir/queryoutput.txt`bir hata atılır.

![Kovan sorgusunun çıktısı](./media/move-hive-tables/output-hive-results-2.png)

Azure Depolama Gezgini'ni kullanarak Hadoop kümesinin varsayılan kapsayıcısını açarsanız, Kovan sorgusunun çıktısını aşağıdaki şekilde gösterildiği gibi görebilirsiniz. Filtreyi (kırmızı kutuyla vurgulanmış) yalnızca adlarda belirtilen harflerle blob almak için uygulayabilirsiniz.

![Hive sorgusunun çıktısını gösteren Azure Depolama Gezgini](./media/move-hive-tables/output-hive-results-3.png)

### <a name="submit-hive-queries-with-the-hive-editor"></a><a name="hive-editor"></a>Hive Düzenleyicisi ile Hive sorguları gönder
Https formunun URL'sini girerek Sorgu Konsolu'nu (Hive Editor) de *kullanabilirsiniz:\//\<Hadoop küme adı>.azurehdinsight.net/Home/HiveEditor* web tarayıcısına. Bu konsolu gör'de oturum açmış olmanız gerekir ve bu nedenle Hadoop küme kimlik bilgilerinize buradan ihtiyacınız olur.

### <a name="submit-hive-queries-with-azure-powershell-commands"></a><a name="ps"></a>Azure PowerShell Komutları ile Kovan sorguları gönderme
Hive sorguları göndermek için PowerShell'i de kullanabilirsiniz. Talimatlar için [PowerShell'i kullanarak Kovan Gönder işleri'ne](../../hdinsight/hadoop/apache-hadoop-use-hive-powershell.md)bakın.

## <a name="create-hive-database-and-tables"></a><a name="create-tables"></a>Kovan veritabanı ve tablolar oluşturma
Hive sorguları [GitHub deposunda](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) paylaşılır ve buradan indirilebilir.

Burada bir Hive tablosu oluşturan Hive sorgusudur.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Takmanız gereken alanların açıklamaları ve diğer yapılandırmalar şunlardır:

* **veritabanı\>adı : oluşturmak istediğiniz veritabanının adı. \<** Sadece varsayılan veritabanını kullanmak istiyorsanız, "*create database...*" sorgusu atlanabilir.
* **tablo\>adı : belirtilen veritabanı içinde oluşturmak istediğiniz tablonun adı. \<** Varsayılan veritabanını kullanmak istiyorsanız, tablo veritabanı adı olmadan \<doğrudan * \<tablo adı\> * \>ile yönlendirilebilir.
* **alan ayırıcı\>: Hive tablosuna yüklenecek veri dosyasındaki alanları sınırlayan ayırıcı. \<**
* **satır ayırıcı\>: veri dosyasındaki çizgileri sınırdışı eden ayırıcı. \<**
* **depolama\>konumu : Hive tablolarının verilerini kaydetmek için Azure Depolama konumu. \<** *KONUM \<depolama konumunu\>* belirtmezseniz, veritabanı ve tablolar *kovan/ambar/dizinde* varsayılan olarak Kovan kümesinin varsayılan kapsayıcısında depolanır. Depolama konumunu belirtmek istiyorsanız, depolama konumu veritabanı ve tablolar için varsayılan kapsayıcı içinde olmalıdır. Bu konum, kümenin varsayılan kapsayıcısına göre *'wasb:///\<dizini 1>/'* veya *'wasb:///\<dizini 1\<>/ dizini 2>/'* vb. biçiminde konum olarak adlanmalıdır. Sorgu yürütüldükten sonra, nispi dizinler varsayılan kapsayıcı içinde oluşturulur.
* **TBLPROPERTIES("skip.header.line.count"="1")**: Veri dosyasında üstbilgi satırı varsa, bu özelliği tablo *sorgusunun* **sonuna** eklemeniz gerekir. Aksi takdirde, üstbilgi satırı tabloya bir kayıt olarak yüklenir. Veri dosyasında üstbilgi satırı yoksa, bu yapılandırma sorguda atlanabilir.

## <a name="load-data-to-hive-tables"></a><a name="load-data"></a>Verileri Kovan tablolarına yükleme
Burada, verileri bir Hive tablosuna yükleyen Hive sorgusu ver.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* **blob veri\>yolu : Hive tablosuna yüklenecek blob dosyası HDInsight Hadoop kümesinin varsayılan kapsayıcısında ysa, blob verilerine giden yol 'bu kapta wasb:// dizin>/ blob dosya adı>' biçiminde olmalıdır. \<** * \<\> * *\<\<* Blob dosyası hdinsight Hadoop kümesinin ek bir kapsayıcı da olabilir. Bu durumda * \<blob verilerine\> giden yol* *'wasb://\<kapsayıcı \<adı>depolama hesabı adı>.blob.core.windows.net/\<blob dosya adı>'* biçiminde olmalıdır.

  > [!NOTE]
  > Hive tablosuna yüklenecek blob verilerinin Hadoop kümesi için depolama hesabının varsayılan veya ek kapsayıcısında olması gerekir. Aksi takdirde, *LOAD DATA* sorgusu verilere erişemediğinden şikayet etmez.
  >
  >

## <a name="advanced-topics-partitioned-table-and-store-hive-data-in-orc-format"></a><a name="partition-orc"></a>Gelişmiş konular: bölümlenmiş tablo ve orc formatında hive verileri depolamak
Veriler büyükse, tablonun bölümlemi yalnızca tablonun birkaç bölümütelemeniz gereken sorgular için yararlıdır. Örneğin, bir web sitesinin günlük verilerini tarihlere göre bölmek makuldür.

Hive tablolarını bölümlemenin yanı sıra, Hive verilerini Optimize Edilmiş Satır Sütunu (ORC) biçiminde depolamak da yararlıdır. ORC biçimlendirme hakkında daha fazla bilgi için <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">bkz.</a>

### <a name="partitioned-table"></a>Bölümlenmiş tablo
Burada, bölümlenmiş bir tablo oluşturan ve veri yükleyen Hive sorgusu ve bu sorgu da burada dır.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Bölümlenmiş tabloları sorgularken, arama verimliliğini artıran `where` yan tümcenin **başına** bölüm koşulu eklenmesi önerilir.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="store-hive-data-in-orc-format"></a><a name="orc"></a>Kovan verilerini ORC formatında depolama
Blob depolamadan gelen verileri doğrudan ORC biçiminde depolanan Kovan tablolarına yükleyemezsiniz. Azure lekelerinden ORC formatında depolanan Hive tablolarına veri yüklemek için yapmanız gereken adımlar aşağıda veda edebilirsiniz.

**TEXTFILE OLARAK DEPOLANAN** harici bir tablo oluşturun ve blob depolamadan tabloya veri yükleyin.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Adım 1'deki dış tabloyla aynı şemaya sahip, aynı alan sınırlayıcısı olan bir iç tablo oluşturun ve Kovan verilerini ORC biçiminde depolayın.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Adım 1'deki dış tablodan verileri seçin ve ORC tablosuna ekleyin

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> EĞER TEXTFILE * \<tablo\>veritabanı\< adı . dış textfile\> tablo adı* bölümleri vardır, `SELECT * FROM <database name>.<external textfile table name>` STEP 3, komut döndürülen veri kümesinde bir alan olarak bölüm değişkeni seçer. *Veritabanı adına\>ekleme.\< \< ORC tablo\> adı* veritabanı adı * \<\>beri başarısız olur.\< ORC tablo\> adı,* tablo şemasında alan olarak bölüm değişkenine sahip değildir. Bu durumda, * \<veritabanı adına\>eklenecek alanları özellikle seçmeniz gerekir.\< ORC tablo\> adı* aşağıdaki gibidir:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Tüm * \<\> * veriler * \<veritabanı adına\>ekildikten sonra aşağıdaki sorguyu kullanırken dış metin dosyası\< tablo adını bırakmak güvenlidir. ORC tablo\>adı*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Bu yordamı uyguladıktan sonra, ORC formatında kullanıma hazır verileri içeren bir tablonuz olmalıdır.  
