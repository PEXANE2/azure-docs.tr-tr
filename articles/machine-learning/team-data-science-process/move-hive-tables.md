---
title: Hive tabloları oluşturma ve Blob depolama alanından - Team Data Science Process veri yükleme
description: Hive tabloları oluşturma ve Azure blob depolamadan veri yükleme için Hive sorguları kullanın. Hive tablolarını bölümlemek ve en iyi duruma getirilmiş satır sütunlu (sorgu performansını artırmak için biçimlendirme ORC) kullanın.
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
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722535"
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Hive tabloları oluşturma ve Azure Blob depolamadan veri yükleme

Bu makalede, Hive tabloları oluşturma ve Azure blob depolamadan veri yükleme genel Hive sorguları gösterir. Hive tablolarını bölümleme ve en iyi duruma getirilmiş satır sütunlu (sorgu performansını artırmak için biçimlendirme ORC) kullanarak, bazı yönergeler de sağlanır.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, olduğunu varsayar:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz. [Azure depolama hesapları hakkında](../../storage/common/storage-introduction.md).
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi hazırlandı.  Yönergelere ihtiyacınız varsa bkz. [HDInsight 'Ta kümeleri ayarlama](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).
* Kümeye uzaktan erişimin etkinleştirilmesi, oturum ve Hadoop komut satırı konsolu açılır. Yönergelere ihtiyacınız varsa bkz. [Apache Hadoop kümelerini yönetme](../../hdinsight/hdinsight-administer-use-portal-linux.md).

## <a name="upload-data-to-azure-blob-storage"></a>Azure blob depolama alanına veri yükleme
Gelişmiş analizler [için bir Azure sanal makinesi ayarlama](../../machine-learning/data-science-virtual-machine/overview.md)bölümünde sunulan yönergeleri Izleyerek bir Azure sanal makinesi oluşturduysanız, bu betik dosyası *C:\\kullanıcılar \<kullanıcı adı\>\\belgeler\\sanal makinedeki veri bilimi betikleri dizini\\* . Bu Hive sorguları yalnızca, gönderim için hazırlamak üzere uygun alanlara bir veri şeması ve Azure Blob depolama yapılandırması sağlamanızı gerektirir.

Hive tablolarının verilerinin **sıkıştırılmamış** tablosal biçiminde olduğunu ve verilerin Hadoop kümesi tarafından kullanılan depolama hesabının varsayılan (veya ek) kapsayıcısına yüklendiğini varsaytık.

**NYC Vergileni seyahat verilerinde**uygulama yapmak istiyorsanız şunları yapmanız gerekir:

* 24 [NYC TAXI seyahat veri](https://www.andresmh.com/nyctaxitrips) dosyalarını **Indirin** (12 seyahat dosyası ve 12 tarifeli havayolu dosyası),
* tüm dosyaları. csv dosyalarına **ayıklayın** ve ardından
* Bunları Azure Storage hesabının varsayılan (veya uygun kapsayıcısına) birine **yükleyin** ; Böyle bir hesabın seçenekleri [Azure HDInsight kümeleri Ile Azure depolama kullanma](../../hdinsight/hdinsight-hadoop-use-blob-storage.md) konusunda görünür. . Csv dosyalarını depolama hesabındaki varsayılan kapsayıcıya yükleme işlemi bu [sayfada](hive-walkthrough.md#upload)bulunabilir.

## <a name="submit"></a>Hive sorguları gönderme
Hive sorgularını kullanarak gönderilebilir:

* [Hadoop kümesinin baş düğümüne 'daki Hadoop komut satırı aracılığıyla Hive sorguları gönderme](#headnode)
* [Hive Düzenleyicisi ile Hive sorguları gönderme](#hive-editor)
* [Azure PowerShell komutlarla Hive sorguları gönderme](#ps)

Hive sorguları SQL benzeri. SQL hakkında bilginiz varsa, [SQL kullanıcıları Için Hive sayfasını](https://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) yararlı bulabilirsiniz.

Bir Hive sorgusu gönderirken ekranda veya baş düğümü üzerindeki yerel bir dosyaya veya bir Azure blob'a oluşmasından Hive sorguları çıkışının hedef de denetleyebilirsiniz.

### <a name="headnode"></a>Hadoop kümesinin baş düğümüne 'daki Hadoop komut satırı aracılığıyla Hive sorguları gönderme
Hive sorgusu karmaşıksa, doğrudan Hadoop baş düğümünde gönderme küme genellikle daha hızlı Hive Düzenleyicisi ya da Azure PowerShell betikleri ile gönderme daha geri dönüş için yol açar.

Hadoop kümesinin baş düğümünde oturum açın, baş düğümün masaüstündeki Hadoop komut satırını açın ve komut `cd %hive_home%\bin`girin.

Size, Hadoop komut satırı Hive sorguları göndermek için üç yolunuz vardır:

* doğrudan
* '. HQL ' dosyalarını kullanma
* Hive Komut Konsolu ile

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Hive sorguları doğrudan, Hadoop komut satırı gönderin.
Basit Hive sorgularını doğrudan Hadoop komut satırında göndermek için `hive -e "<your hive query>;` gibi komutu çalıştırabilirsiniz. Burada kırmızı kutu Hive sorgusu gönderen komut özetlemekte ve Hive sorgusu çıkışı yeşil kutuyu özetler bir örnek aşağıda verilmiştir.

![Hive sorgusu çıkışı ile Hive sorgu göndermek için komutu](./media/move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>'. HQL ' dosyalarında Hive sorguları gönder
Komut satırı ya da Hive komut konsolunda sorguları düzenleme, Hive sorgusu daha karmaşıktır ve birden fazla satır olduğunda pratik değildir. Diğer bir seçenek de, Hadoop kümesinin baş düğümünde bir metin düzenleyicisi kullanarak Hive sorgularını baş düğümün yerel dizinindeki bir '. HQL ' dosyasına kaydeder. Ardından, '. HQL ' dosyasındaki Hive sorgusu, `-f` bağımsız değişkeni kullanılarak şu şekilde gönderilebilir:

    hive -f "<path to the '.hql' file>"

!['. HQL ' dosyasında Hive sorgusu](./media/move-hive-tables/run-hive-queries-3.png)

**İlerleme durumu ekranını gizle Hive sorgularının yazdırılması**

Hive sorgusu, Hadoop komut satırında gönderildikten sonra varsayılan olarak, Map/Reduce işinin ilerleme durumunu ekranda yazdırılır. Harita/iş ilerlemesini azaltma ekranını bastırmak için, komut satırında aşağıdaki gibi `-S` bir bağımsız değişken (büyük harfle "S") kullanabilirsiniz:

    hive -S -f "<path to the '.hql' file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Hive komut konsolunda Hive sorguları göndermek.
Ayrıca, Hadoop komut satırında komut `hive` çalıştırarak Hive komut konsolunu girip Hive sorguları Hive komut konsoluna gönderebilirsiniz. Bir örnek aşağıda verilmiştir. Bu örnekte, iki kırmızı kutuları, Hive komut konsoluna girmek için kullanılan komutlar ve Hive komut konsolunda sırasıyla gönderilen Hive sorgusu vurgulayın. Yeşil kutuyu Hive sorgusu çıkışı vurgular.

![Hive komut konsolunu açın ve komutu girin, Hive sorgusu çıkışı görüntülemek](./media/move-hive-tables/run-hive-queries-2.png)

Önceki örneklerde, ekranda Hive sorgu sonuçları doğrudan çıkış. Yerel bir dosyaya baş düğüme veya bir Azure blob çıktı yazabilirsiniz. Ardından, Hive sorguları çıkışı daha fazla analiz için diğer araçları kullanabilirsiniz.

**Çıktı Hive sorgusu sonuçları yerel bir dosyaya göre yapılır.**
Hive sorgu sonuçlarını baş düğümü üzerindeki yerel bir dizine çıkarmak için Hive sorgusunu, Hadoop komut satırını aşağıdaki şekilde gönderin vardır:

    hive -e "<hive query>" > <local path in the head node>

Aşağıdaki örnekte, Hive sorgusunun çıktısı Dizin `C:\apps\temp``hivequeryoutput.txt` bir dosyaya yazılır.

![Hive sorgusu çıkışı](./media/move-hive-tables/output-hive-results-1.png)

**Çıktı Hive sorgusu sonuçları bir Azure Blob 'una**

Ayrıca, Hadoop kümesi varsayılan kapsayıcı içinde bir Azure blob'a Hive sorgu sonuçları çıkış sağlayabilir. Bu Hive sorgu aşağıdaki gibidir:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Aşağıdaki örnekte, Hive sorgusunun çıktısı, Hadoop kümesinin varsayılan kapsayıcısı içinde `queryoutputdir` bir blob dizinine yazılır. Burada, yalnızca blob adı olmayan bir dizin adı sağlamanız gerekir. `wasb:///queryoutputdir/queryoutput.txt`gibi hem dizin hem de blob adları sağlarsanız bir hata oluşur.

![Hive sorgusu çıkışı](./media/move-hive-tables/output-hive-results-2.png)

Azure Depolama Gezgini'ni kullanarak Hadoop kümesi varsayılan kapsayıcı açarsanız, aşağıdaki resimde gösterildiği gibi Hive sorgusu çıkışı görebilirsiniz. Yalnızca blob adları belirtilen harflerle almak için (kırmızı kutu ile vurgulanan) filtre uygulayabilirsiniz.

![Hive sorgusu çıkışı gösteren Azure Depolama Gezgini](./media/move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a>Hive Düzenleyicisi ile Hive sorguları gönderme
Ayrıca, bir Web tarayıcısına *https:\//\<Hadoop kümesi adı >. azurehdinsight. net/Home/HiveEditor* BIÇIMINDE bir URL girerek sorgu konsolunu (Hive Düzenleyicisi) kullanabilirsiniz. Siz bu konsolun bakın oturum ve Hadoop kümesi kimlik bilgilerinizi buraya nedenle gerekir.

### <a name="ps"></a>Azure PowerShell komutlarla Hive sorguları gönderme
Hive sorguları göndermek için PowerShell de kullanabilirsiniz. Yönergeler için bkz. [PowerShell kullanarak Hive Işleri gönderme](../../hdinsight/hadoop/apache-hadoop-use-hive-powershell.md).

## <a name="create-tables"></a>Hive veritabanı ve tabloları oluşturma
Hive sorguları [GitHub deposunda](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) paylaşılır ve buradan indirilebilir.

Bir Hive tablosu oluşturur Hive sorgusu aşağıda verilmiştir.

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

Açıklamaları takın gereken alanların ve diğer yapılandırmalar şunlardır:

* **\<veritabanı adı\>** : oluşturmak istediğiniz veritabanının adı. Yalnızca varsayılan veritabanını kullanmak istiyorsanız, "*veritabanı oluştur...* " sorgusu atlanabilir.
* **\<tablo adı\>** : belirtilen veritabanı içinde oluşturmak istediğiniz tablonun adı. Varsayılan veritabanını kullanmak istiyorsanız, tablo \<veritabanı\>adı olmadan *\>\<tablo adı* ile doğrudan başvuruda bulunabilir.
* **\<alan ayırıcısı\>** : veri dosyasında Hive tablosuna yüklenecek alanları sınırlandıran ayırıcı.
* **\<satır ayırıcı\>** : veri dosyasındaki satırları sınırlandıran ayırıcı.
* **\<depolama konumu\>** : Hive tablolarının verilerini kaydetmek Için Azure depolama konumu. *\<depolama konumu\>* belirtmezseniz, veritabanı ve tablolar varsayılan olarak Hive kümesinin varsayılan kapsayıcısında *Hive/ambar/* dizinde depolanır. Depolama konumu belirtmek istiyorsanız, depolama konumu, tablo ve veritabanı için varsayılan kapsayıcı içinde olması gerekir. Bu konumun, *' wasb:///\<Directory 1 >/'* veya *' wasb:///\<Directory 1 >/\<Directory 2 >/'* vb. biçimindeki kümenin varsayılan kapsayıcısına göre konum olarak bahsedilmelidir. Sorgu yürütüldükten sonra, ilişkili dizinler varsayılan kapsayıcı içinde oluşturulur.
* **TBLPROPERTIES ("Skip. Header. Line. Count" = "1")** : veri dosyasında bir başlık satırı varsa, bu özelliği *Create Table* sorgusunun **sonuna** eklemeniz gerekir. Aksi takdirde, üstbilgi satırını tablosuna bir kayıt olarak yüklenir. Veri dosyasındaki bir üst bilgi satırı yoksa, bu yapılandırma sorguda atlanmış olabilir.

## <a name="load-data"></a>Hive tablolarına veri yükleme
Bir Hive tablosuna veri yükler Hive sorgusu aşağıda verilmiştir.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* **BLOB veri\>yolu\<** : Hive tablosuna yüklenecek blob dosyası HDInsight Hadoop kümesinin varsayılan kapsayıcıda ise, *blob veri\>\<yolu* *bu kapsayıcıdaki ' wasb://\<dizininde yer almalıdır >/\<BLOB dosya adı > '* . Blob dosya HDInsight Hadoop kümesi ek bir kapsayıcı da olabilir. Bu durumda, *BLOB veri\>yolu\<* *' wasb://\<kapsayıcı adı >\<depolama hesabı adı >. blob. Core. Windows. net/\<blob dosya adı > '* biçiminde olmalıdır.

  > [!NOTE]
  > Hive tablosu yüklenmek üzere blob verilerini varsayılan veya ek kapsayıcı Hadoop kümesi için depolama hesabının olması gerekir. Aksi halde, veri *yükleme* sorgusu, verilere erişemediği konusunda şikayetçi olur.
  >
  >

## <a name="partition-orc"></a>Gelişmiş Konular: bölümlenmiş tablo ve Hive verilerini ORC biçiminde depola
Veriler büyükse, tablo bölümleme yalnızca tablo bazı bölümleri taraması gereken sorguları için yararlıdır. Örneğin, bir web sitesi günlük verilerini tarihlerine göre bölümlemek şüphelenilebilir.

Hive tablolarını bölümleme yanı sıra en iyi duruma getirilmiş satır sütunlu (ORC) biçiminde Hive verileri depolamak faydalı olacaktır. ORC biçimlendirmesi hakkında daha fazla bilgi için, bkz. <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">orc dosyaları kullanma, Hive verileri okurken, yazarken ve işlerken performansı geliştirir</a>.

### <a name="partitioned-table"></a>Bölümlenmiş bir tablo
Bölümlenmiş bir tablo oluşturur ve verileri içine yükler Hive sorgusunu aşağıda verilmiştir.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Bölümlenmiş tabloları sorgularken, Bölüm koşulunun `where` yan tümcesinin **başına** eklenmesi önerilir ve bu, arama verimliliğini artırır.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Hive verilerini ORC biçiminde depolayın
ORC biçiminde depolanan Hive tablolarına, blob depolama alanından verileri doğrudan yüklenemiyor. , Yüklemek için uygulamanız gereken adımlar şunlardır blobları Azure verileri ORC biçiminde depolanan Hive tablolarına.

**TEXTFILE olarak depolanan** bir dış tablo oluşturun ve BLOB depolamadan tabloya veri yükleyin.

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

Dış tablo adım 1, aynı alan sınırlayıcı ile aynı şemaya sahip iç tablo oluşturun ve Hive verileri ORC biçiminde depolar.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

1\. adım dış tablodaki verileri seçin ve ORC tabloya Ekle

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> TEXTFILE tablosu *\<veritabanı adı\>.\<dış textfile tablo adı\>* bölümler IÇERIYORSA, adım 3 ' te `SELECT * FROM <database name>.<external textfile table name>` komutu, Bölüm değişkenini döndürülen veri kümesindeki bir alan olarak seçer. *\<veritabanı adına ekleme\>.\<ORC tablo adı\>* \<veritabanı adı\>bu yana başarısız olur *.\<orc tablo adı\>* tablo şemasında bir alan olarak bölüm değişkenine sahip değildir. Bu durumda,\<veritabanı adına eklenecek alanları özel olarak seçmeniz gerekir *\>.\<ORC tablo adı\>* aşağıdaki gibidir:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Tüm veriler\<veritabanı adına eklendikten sonra aşağıdaki sorgu kullanılırken *\>dış metin dosyası tablo adının\<* serbest bırakması güvenlidir *\>.\<orc tablo adı\>* :

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Bu yordamı tamamladıktan sonra verileri ORC biçiminde kullanıma hazır bir tablo olmalıdır.  
