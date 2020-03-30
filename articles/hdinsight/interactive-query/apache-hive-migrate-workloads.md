---
title: Azure HDInsight 3.6 Kovan iş yüklerini HDInsight 4.0'a geçirin
description: HDInsight 3.6'daki Apache Hive iş yüklerini HDInsight 4.0'a nasıl geçireceğinizi öğrenin.
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: ec96189185a06c1fcbd95eed6216ade47f3089c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214657"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Azure HDInsight 3.6 Kovan iş yüklerini HDInsight 4.0'a geçirin

Bu belge, HDInsight 3.6'daki Apache Hive ve LLAP iş yüklerinin nasıl HDInsight 4.0'a geçirilen bir yol olduğunu gösterir. HDInsight 4.0, daha yeni Hive ve LLAP özellikleri sağlar, örneğin somutlaştırılmış görünümler ve sorgu sonucu önbelleğe alma. İş yüklerinizi HDInsight 4.0'a geçirdiğinizde, Hive 3'ün HDInsight 3.6'da bulunmayan birçok yeni özelliğini kullanabilirsiniz.

Bu makale aşağıdaki konuları kapsamaktadır:

* Hive meta verilerinin HDInsight 4.0'a geçişi
* ASİt ve ASİt OLMAYAN tabloların güvenli geçişi
* HDInsight sürümlerinde Hive güvenlik politikalarının korunması
* HDInsight 3.6'dan HDInsight 4.0'a yürütme yi ve hata ayıklama sorgula

Hive'ın bir avantajı, meta verileri harici bir veritabanına (Kovan Metastore olarak anılacaktır) dışa aktarma yeteneğidir. **Hive Metastore,** tablo depolama konumu, sütun adları ve tablo dizini bilgileri de dahil olmak üzere tablo istatistiklerini depolamaktan sorumludur. Metastore veritabanı şeması Hive sürümleri arasında farklılık gösterir. Hive metastore'u güvenli bir şekilde yükseltmenin önerilen yolu, geçerli üretim ortamı yerine bir kopyasını oluşturmak ve kopyayı yükseltmektir.

## <a name="copy-metastore"></a>Metastore'u kopyala

HDInsight 3.6 ve HDInsight 4.0 farklı metastore şemaları gerektirir ve tek bir metastore'u paylaşamaz.

### <a name="external-metastore"></a>Dış metastore

Harici metastore'unuzun yeni bir kopyasını oluşturun. Harici bir metastore kullanıyorsanız, metastore'un bir kopyasını oluşturmanın güvenli ve kolay yollarından biri, SQL Veritabanı geri yükleme işlevini kullanarak [Veritabanı'nı](../../sql-database/sql-database-recovery-using-backups.md#point-in-time-restore) farklı bir adla geri yüklemektir.  Bkz. Bir HDInsight kümesine harici bir metastore ekleme hakkında daha fazla bilgi edinmek için [Azure HDInsight'taki harici meta veri depolarını kullanın.](../hdinsight-use-external-metadata-stores.md)

### <a name="internal-metastore"></a>Dahili metastore

İç metastore kullanıyorsanız, sorguları Hive metamağazasında nesne tanımlarını dışa aktarmak ve bunları yeni bir veritabanına aktarmak için kullanabilirsiniz.

Bu komut dosyası tamamlandıktan sonra, eski kümenin artık komut dosyasında belirtilen tablolara veya veritabanlarından hiçbirine erişmek için kullanılmayacağı varsayılır.

> [!NOTE]
> ACID tabloları söz konusu olduğunda, tablonun altındaki verilerin yeni bir kopyası oluşturulur.

1. [Güvenli Kabuk (SSH) istemcisi](../hdinsight-hadoop-linux-use-ssh-unix.md)kullanarak HDInsight kümesine bağlanın.

1. Aşağıdaki komutu girerek açık SSH oturumunuzdan [Beeline istemcinizle](../hadoop/apache-hadoop-use-hive-beeline.md) HiveServer2'ye bağlanın:

    ```hiveql
    for d in `beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show databases;"`; 
    do
        echo "Scanning Database: $d"
        echo "create database if not exists $d; use $d;" >> alltables.hql; 
        for t in `beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show tables;"`;
        do
            echo "Copying Table: $t"
            ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;"`;

            echo "$ddl;" >> alltables.hql;
            lowerddl=$(echo $ddl | awk '{print tolower($0)}')
            if [[ $lowerddl == *"'transactional'='true'"* ]]; then
                if [[ $lowerddl == *"partitioned by"* ]]; then
                    # partitioned
                    raw_cols=$(beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;" | tr '\n' ' ' | grep -io "CREATE TABLE .*" | cut -d"(" -f2- | cut -f1 -d")" | sed 's/`//g');
                    ptn_cols=$(beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;" | tr '\n' ' ' | grep -io "PARTITIONED BY .*" | cut -f1 -d")" | cut -d"(" -f2- | sed 's/`//g');
                    final_cols=$(echo "(" $raw_cols "," $ptn_cols ")")

                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "create external table ext_$t $final_cols TBLPROPERTIES ('transactional'='false');";
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "insert into ext_$t select * from $t;";
                    staging_ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table ext_$t;"`;
                    dir=$(echo $staging_ddl | grep -io " LOCATION .*" | grep -m1 -o "'.*" | sed "s/'[^-]*//2g" | cut -c2-);

                    parsed_ptn_cols=$(echo $ptn_cols| sed 's/ [a-z]*,/,/g' | sed '$s/\w*$//g');
                    echo "create table flattened_$t $final_cols;" >> alltables.hql;
                    echo "load data inpath '$dir' into table flattened_$t;" >> alltables.hql;
                    echo "insert into $t partition($parsed_ptn_cols) select * from flattened_$t;" >> alltables.hql;
                    echo "drop table flattened_$t;" >> alltables.hql;
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "drop table ext_$t";
                else
                    # not partitioned
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "create external table ext_$t like $t TBLPROPERTIES ('transactional'='false');";
                    staging_ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table ext_$t;"`;
                    dir=$(echo $staging_ddl | grep -io " LOCATION .*" | grep -m1 -o "'.*" | sed "s/'[^-]*//2g" | cut -c2-);

                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "insert into ext_$t select * from $t;";
                    echo "load data inpath '$dir' into table $t;" >> alltables.hql;
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "drop table ext_$t";
                fi
            fi
            echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t;" >> alltables.hql;
        done;
    done
    ```

    Bu komut **alltables.hql**adlı bir dosya oluşturur.

1. SSH oturumundan çıkın. Sonra **alltables.hql** yerel indirmek için bir scp komutu girin.

    ```bash
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:alltables.hql c:/hdi
    ```

1. **Alltables.hql'u** *yeni* HDInsight kümesine yükleyin.

    ```bash
    scp c:/hdi/alltables.hql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

1. Ardından *yeni* HDInsight kümesine bağlanmak için SSH'yi kullanın. SSH oturumundan aşağıdaki kodu çalıştırın:

    ```bash
    beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" -i alltables.hql
    ```


## <a name="upgrade-metastore"></a>Yükseltme metastore

Metastore **kopyası** tamamlandıktan sonra, yeni metastore'u Hive 3 şemasına yükseltmek için mevcut HDInsight 3.6 kümesinde [Script Action'da](../hdinsight-hadoop-customize-cluster-linux.md) bir şema yükseltme komut dosyası çalıştırın. Bu, veritabanının HDInsight 4.0 metastore olarak eklenmesini sağlar.

Aşağıdaki tablodaki değerleri kullanın. `SQLSERVERNAME DATABASENAME USERNAME PASSWORD` **Boşluklara ayrılmış, kopyalanan** Kovan metadeposu için uygun değerleri değiştirin. SQL sunucu adını belirtirken ".database.windows.net" eklemeyin.

|Özellik | Değer |
|---|---|
|Komut dosyası türü|- Özel|
|Adı|Kovan yükseltme|
|Bash script URI|`https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/launch-schema-upgrade.sh`|
|Düğüm türü(ler)|Head|
|Parametreler|SQLSERVERNAME DATABASENAME Kullanıcı Adı ŞIFRE|

> [!Warning]  
> HDInsight 3.6 meta veri şemasını HDInsight 4.0 şemasına dönüştüren yükseltme geri alınamaz.

Aşağıdaki sql sorgusunu veritabanına karşı çalıştırarak yükseltmeyi doğrulayabilirsiniz:

```sql
select * from dbo.version
```

## <a name="migrate-hive-tables-to-hdinsight-40"></a>Hive tablolarını HDInsight 4.0'a geçirin

Hive Metastore'u HDInsight 4.0'a geçirmek için önceki adımları tamamladıktan sonra, metastore'da kaydedilen tablolar ve veritabanları, hdinsight `show tables` 4.0 kümesinin içinden veya kümenin `show databases` içinden görünür. HDInsight 4.0 kümelerinde sorgu yürütme hakkında bilgi için [HDInsight sürümlerinde Sorgu](#query-execution-across-hdinsight-versions) yürütmesi'ne bakın.

Ancak, küme gerekli depolama hesaplarına erişene kadar tablolardaki gerçek verilere erişilemez. HDInsight 4.0 kümenizin eski HDInsight 3.6 kümenizle aynı verilere erişebilmesini sağlamak için aşağıdaki adımları tamamlayın:

1. Tablonuzun veya veritabanınızın Azure depolama hesabını belirleyin.

1. HDInsight 4.0 kümeniz zaten çalışıyorsa, Ambari aracılığıyla Azure depolama hesabını kümeye takın. HDInsight 4.0 kümesini henüz oluşturmadıysanız, Azure depolama hesabının birincil veya ikincil küme depolama hesabı olarak belirtildiğinden emin olun. HDInsight kümelerine depolama hesabı ekleme hakkında daha fazla bilgi için [bkz.](../hdinsight-hadoop-add-storage.md)

## <a name="deploy-new-hdinsight-40-and-connect-to-the-new-metastore"></a>Yeni HDInsight 4.0'ı dağıtın ve yeni metastore'a bağlanın

Şema yükseltmesi tamamlandıktan sonra, yeni bir HDInsight 4.0 kümesi ni dağıtın ve yükseltilmiş metastore'u bağlayın. 4.0'ı zaten dağıttıysanız, Ambari'deki metastore'a bağlanabilmeniz için ayarlayın.

## <a name="run-schema-migration-script-from-hdinsight-40"></a>HDInsight 4.0'dan şema geçiş komut dosyalarını çalıştırma

HDInsight 3.6 ve HDInsight 4.0'da tablolar farklı şekilde ele alınır. Bu nedenle, farklı sürümkümeleri için aynı tabloları paylaşamazsınız. HDInsight 3.6'yı HDInsight 4.0 ile aynı anda kullanmak istiyorsanız, her sürüm için verilerin ayrı kopyalarına sahip olmalısınız.

Kovan iş yükü ASİt ve asitsiz tabloların bir karışımını içerebilir. HDInsight 3.6 'da (Hive 2) Hive ile HDInsight 4.0'da (Hive 3) Hive arasındaki temel farklardan biri tablolar için ASİt uyumluluğudur. HDInsight 3.6'da, Kovan ASİt uyumluluğunu etkinleştirmek ek yapılandırma gerektirir, ancak HDInsight 4.0 tablolarında varsayılan olarak ASİt uyumluolur. Geçişten önce gereken tek eylem, 3.6 kümesindeki ACID tablosuna karşı büyük bir sıkıştırma çalıştırmaktır. Hive görünümünden veya Beeline'den aşağıdaki sorguyu çalıştırın:

```sql
alter table myacidtable compact 'major';
```

HDInsight 3.6 ve HDInsight 4.0 ACID tabloları ACID deltas'ı farklı anladığı için bu sıkıştırma gereklidir. Sıkıştırma, tutarlılığı garanti eden temiz bir sayfa uygular. [Hive geçiş belgelerinin](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-upgrade-major/content/prepare_hive_for_upgrade.html) Bölüm 4'ü HDInsight 3.6 ACID tablolarının toplu sıkıştırma kılavuzunu içerir.

Metastore geçiş ve sıkıştırma adımlarını tamamladıktan sonra, gerçek ambarı geçirebilirsiniz. Hive ambarı geçişini tamamladıktan sonra, HDInsight 4.0 ambarı aşağıdaki özelliklere sahip olacaktır:

|3.6 |4.0 |
|---|---|
|Dış tablolar|Dış tablolar|
|İşlem dışı yönetilen tablolar|Dış tablolar|
|İşlemsel yönetilen tablolar|Yönetilen tablolar|

Geçişi yürütmeden önce ambarınızın özelliklerini ayarlamanız gerekebilir. Örneğin, bazı tabloya üçüncü bir taraf (HDInsight 3.6 kümesi gibi) erişmesini bekliyorsanız, geçiş tamamlandıktan sonra bu tablonun dışsal olması gerekir. HDInsight 4.0'da yönetilen tüm tablolar işlemseldir. Bu nedenle, HDInsight 4.0'daki yönetilen tablolara yalnızca HDInsight 4.0 kümeleri tarafından erişilmelidir.

Tablo özellikleriniz doğru ayarlandıktan sonra, SSH kabuğunu kullanarak küme başlıklarından birinden Kovan ambarı geçiş aracını çalıştırın:

1. SSH kullanarak küme kafa nodonunuza bağlanın. Talimatlar için Bkz. [SSH kullanarak HDInsight'a bağlan](../hdinsight-hadoop-linux-use-ssh-unix.md)
1. Çalıştırarak Hive kullanıcısı olarak giriş kabuğunu açma`sudo su - hive`
1. Yürüterek veri platformu yığını `ls /usr/hdp`sürümünü belirleyin. Bu, bir sonraki komutta kullanmanız gereken bir sürüm dizesini görüntüler.
1. Kabuktan aşağıdaki komutu uygulayın. Önceki `STACK_VERSION` adımdaki sürüm dizesiyle değiştirin:

```bash
/usr/hdp/STACK_VERSION/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true -m automatic --modifyManagedTables
```

Geçiş aracı tamamlandıktan sonra, Kovan ambarınız HDInsight 4.0 için hazır olacaktır.

> [!Important]  
> HDInsight 4.0'daki yönetilen tablolara (3,6'dan geçirilen tablolar dahil) HDInsight 3.6 kümeleri de dahil olmak üzere diğer hizmetler veya uygulamalar tarafından erişilmemelidir.

## <a name="secure-hive-across-hdinsight-versions"></a>HDInsight sürümlerinde Güvenli Kovan

HDInsight 3.6'dan bu yana HDInsight, HDInsight Kurumsal Güvenlik Paketi 'ni (ESP) kullanarak Azure Active Directory ile bütünleşir. ESP küme içinde belirli kaynakların izinlerini yönetmek için Kerberos ve Apache Ranger kullanır. HDInsight 3.6'da Hive'a karşı dağıtılan Ranger politikaları aşağıdaki adımlarla HDInsight 4.0'a geçirilebilir:

1. HDInsight 3.6 kümenizdeki Ranger Servis Yöneticisi paneline gidin.
2. **HIVE** adlı ilkeye gidin ve ilkeyi bir json dosyasına dışa aktarın.
3. Dışa aktarılan ilke json'da belirtilen tüm kullanıcıların yeni kümede bulunduğundan emin olun. Bir kullanıcı ilke json'da adı atılıyorsa ancak yeni kümede yoksa, kullanıcıyı yeni kümeye ekleyin veya başvuruyu ilkeden kaldırın.
4. HDInsight 4.0 kümenizdeki **Ranger Service Manager** paneline gidin.
5. **HIVE** adlı ilke gidin ve adım 2 ranger ilkesi json almak.

## <a name="check-compatibility-and-modify-codes-as-needed-in-test-app"></a>Test uygulamasında gerektiğinde uyumluluğu kontrol edin ve kodları değiştirin

Varolan programlar ve sorgular gibi iş yüklerini geçirerek, lütfen sürüm notlarını ve belgelerini değişiklikler için denetleyin ve değişiklikleri gerektiği gibi uygulayın. HDInsight 3.6 kümeniz paylaşılan bir Kıvılcım ve Kovan metastore kullanıyorsa, [Hive Warehouse Konektörü kullanarak ek yapılandırma](./apache-hive-warehouse-connector.md) gereklidir.

## <a name="deploy-new-app-for-production"></a>Üretim için yeni uygulamayı dağıtma

Yeni kümeye geçmek için, örneğin yeni bir istemci uygulaması yükleyebilir ve yeni bir üretim ortamı olarak kullanabilirsiniz veya mevcut istemci uygulamanızı yükseltebilir ve HDInsight 4.0'a geçebilirsiniz.

## <a name="switch-hdinsight-40-to-the-production"></a>HDInsight 4.0'ı üretime geçirin

Sınama sırasında metastore'da farklılıklar oluşturulduysa, geçiş yapmadan hemen önce değişiklikleri güncelleştirmeniz gerekir. Bu durumda, dışa aktarabilir & metastore'u içe aktarAbilir ve sonra yeniden yükseltebilirsiniz.

## <a name="remove-the-old-production"></a>Eski üretimi kaldırın

Sürümün eksiksiz ve tam olarak çalışır durumda olduğunu doğruladıktan sonra, sürüm 3.6 ve önceki metastore'u kaldırabilirsiniz. Lütfen ortamı silmeden önce her şeyin geçirilmiş olduğundan emin olun.

## <a name="query-execution-across-hdinsight-versions"></a>HDInsight sürümlerinde yürütmeyi sorgula

Bir HDInsight 3.6 kümesi nde Hive/LLAP sorgularını yürütmenin ve hata ayıklamanın iki yolu vardır. HiveCLI komut satırı deneyimi sağlar ve Tez görünümü/Hive görünümü GUI tabanlı bir iş akışı sağlar.

HDInsight 4.0'da HiveCLI beeline ile değiştirildi. HiveCLI Hiveserver 1 için bir tutumlu istemcive Beeline Hiveserver 2 erişim sağlayan bir JDBC istemcisidir. Beeline, diğer JDBC uyumlu veritabanı bitiş noktasına bağlanmak için de kullanılabilir. Beeline, HDInsight 4.0'da herhangi bir kurulum alamadan kullanıma hazır olarak kullanılabilir.

HDInsight 3.6'da, Hive sunucusuyla etkileşim kurmak için GUI istemcisi Ambari Hive View'dır. HDInsight 4.0 Ambari View ile gönderilmez. Müşterilerimizin temel bir HDInsight hizmeti olmayan Data Analytics Studio'yu (DAS) kullanmaları için bir yol sağladık. DAS, HDInsight kümeleri ile kutudan çıkmaz ve resmi olarak desteklenen bir paket değildir. Ancak, DAS aşağıdaki gibi bir [komut dosyası eylemi](../hdinsight-hadoop-customize-cluster-linux.md) kullanarak kümeye yüklenebilir:

|Özellik | Değer |
|---|---|
|Komut dosyası türü|- Özel|
|Adı|Demir|
|Bash script URI|`https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh`|
|Düğüm türü(ler)|Head|

10 ila 15 dakika bekleyin, ardından bu URL'yi kullanarak Data Analytics Studio'u başlatın: `https://CLUSTERNAME.azurehdinsight.net/das/`.

Ambari UI'nin yenilenmesi ve/veya tüm Ambari bileşenlerinin yeniden başlatılması DAS'a erişmeden önce gerekli olabilir.

DAS yüklendikten sonra, sorgu görüntüleyicide çalıştırdığınız sorguları görmüyorsanız aşağıdaki adımları yapın:

1. [Sorun giderme DAS yüklemesi için bu kılavuzda](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html)açıklandığı gibi Hive, Tez ve DAS yapılandırmaları ayarlayın.
2. Aşağıdaki Azure depolama dizin configs Sayfa lekeleri olduğundan emin olun ve `fs.azure.page.blob.dirs`bunların altında listelenmiştir:
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. Her iki başlıkta da HDFS, Hive, Tez ve DAS'ı yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight 4.0 Duyurusu](../hdinsight-version-release.md)
* [HDInsight 4.0 derin dalış](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [Kovan 3 ASİt Tablolar](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
