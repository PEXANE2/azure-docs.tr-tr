---
title: Azure HDInsight 3,6 Hive iş yüklerini HDInsight 'a geçirme 4,0
description: HDInsight 3,6 ' de Apache Hive iş yüklerini HDInsight 4,0 ' ye geçirmeyi öğrenin.
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: 9f49a9224ed123b76f4d300c27a8dd5822e50ea3
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74706031"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Azure HDInsight 3,6 Hive iş yüklerini HDInsight 'a geçirme 4,0

Bu belgede HDInsight 3,6 ' de Apache Hive ve LLAP iş yüklerinin HDInsight 4,0 ' ye nasıl geçirileceği gösterilmektedir. HDInsight 4,0, gerçekleştirilmiş görünümler ve sorgu sonucu önbelleği gibi daha yeni Hive ve LLAP özellikleri sağlar. İş yüklerinizi HDInsight 4,0 ' e geçirdiğinizde, Hive 3 ' ün HDInsight 3,6 ' de kullanılamayan daha yeni özelliklerinden yararlanabilirsiniz.

Bu makalede aşağıdaki konular ele alınmaktadır:

* Hive meta verilerinin HDInsight 'a geçirilmesi 4,0
* ACID ve ACID olmayan tabloların güvenli geçişi
* HDInsight sürümleri arasında Hive güvenlik ilkelerinin korunması
* HDInsight 3,6 ' den HDInsight 4,0 ' den sorgu yürütme ve hata ayıklama

Hive 'nin bir avantajı, meta verileri bir dış veritabanına dışarı aktarma (Hive meta veri deposu olarak adlandırılır) özelliğidir. **Hive meta** veri tablosu, tablo depolama konumu, sütun adları ve tablo dizini bilgileri de dahil olmak üzere tablo istatistiklerini depolamaktan sorumludur. Meta veri deposu veritabanı şeması Hive sürümleri arasında farklılık gösterir. Hive meta veri deposu güvenli bir şekilde yükseltmek için önerilen yol, geçerli üretim ortamı yerine bir kopya oluşturmak ve kopyayı yükseltmedir.

## <a name="copy-metastore"></a>Meta veri deposunu Kopyala

HDInsight 3,6 ve HDInsight 4,0 farklı meta veri deposu şemaları gerektirir ve tek bir meta veri deposu paylaşamaz.

### <a name="external-metastore"></a>Dış meta veri deposu

Dış meta veri deposu 'nun yeni bir kopyasını oluşturun. Dış meta veri deposu kullanıyorsanız, meta veri geri yükleme 'nin bir kopyasını oluşturmanın güvenli ve kolay yöntemlerinden biri, SQL veritabanı geri yükleme işlevini kullanarak veritabanını farklı bir adla [geri yüklemektir](../../sql-database/sql-database-recovery-using-backups.md#point-in-time-restore) .  HDInsight kümesine dış meta veri deposu ekleme hakkında daha fazla bilgi edinmek için bkz. [Azure HDInsight 'ta dış meta veri depoları kullanma](../hdinsight-use-external-metadata-stores.md) .

### <a name="internal-metastore"></a>İç meta veri deposu

İç meta veri deposu kullanıyorsanız, Hive meta veri deposu nesne tanımlarını dışarı aktarmak ve yeni bir veritabanına içeri aktarmak için sorguları kullanabilirsiniz.

1. Bir [Secure Shell (SSH) istemcisi](../hdinsight-hadoop-linux-use-ssh-unix.md)kullanarak HDInsight kümesine bağlanın.

1. Aşağıdaki komutu girerek, açık SSH oturumunuzla [Beeline Istemcinizden](../hadoop/apache-hadoop-use-hive-beeline.md) HiveServer2 'e bağlanın:

    ```hiveql
    for d in `beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show databases;"`; do echo "create database $d; use $d;" >> alltables.sql; for t in `beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show tables;"` ; do ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

    Bu komut **alltables. SQL**adlı bir dosya oluşturur. Varsayılan veritabanı silinemediği/yeniden oluşturulamadığı için lütfen **alltables. SQL**' de `create database default;` ifadesini kaldırın.

1. SSH oturumunuzla çıkış yapın. Sonra **alltables. SQL** ' i yerel olarak indirmek için bir SCP komutu girin.

    ```bash
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:alltables.sql c:/hdi
    ```

1. **Alltables. SQL** ' i *Yeni* HDInsight kümesine yükleyin.

    ```bash
    scp c:/hdi/alltables.sql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

1. Ardından, *Yeni* HDInsight kümesine bağlanmak için SSH kullanın. SSH oturumundan aşağıdaki kodu çalıştırın:

    ```bash
    beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" -i alltables.sql
    ```

## <a name="upgrade-metastore"></a>Meta veri deposunu yükselt

Meta veri **kopyalama** işlemi tamamlandıktan sonra, yeni meta veri deposunu Hive 3 şemasına yükseltmek Için mevcut HDInsight 3,6 kümesindeki [komut dosyası eyleminde](../hdinsight-hadoop-customize-cluster-linux.md) bir şema yükseltme betiği çalıştırın. Bu, veritabanının HDInsight 4,0 meta veri deposu olarak eklenmesini sağlar.

Aşağıdaki tablodaki değerleri daha fazla kullanın. `SQLSERVERNAME DATABASENAME USERNAME PASSWORD`, **kopyalanmış** Hive meta veri deposu için boşluklarla ayırarak uygun değerlerle değiştirin. SQL Server adını belirtirken ". database.windows.net" eklemeyin.

|Özellik | Değer |
|---|---|
|Betik türü|-Özel|
|Adı|Hive yükseltmesi|
|Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/launch-schema-upgrade.sh`|
|Düğüm türleri|Baş|
|Parametreler|SQLSERVERNAME DATABASENAME KULLANıCı ADı PAROLASı|

> [!Warning]  
> HDInsight 3,6 meta veri şemasını HDInsight 4,0 şemasına dönüştüren yükseltme, geri alınamaz.

Veritabanında aşağıdaki SQL sorgusunu çalıştırarak yükseltmeyi doğrulayabilirsiniz:

```sql
select * from dbo.version
```

## <a name="migrate-hive-tables-to-hdinsight-40"></a>Hive tablolarını HDInsight 4,0 'e geçirme

Hive meta veri deposunu HDInsight 4,0 ' ye geçirmeye yönelik önceki adım kümesini tamamladıktan sonra, meta veri deposu 'nda kaydedilen tablolar ve veritabanları, `show tables` veya küme içinden `show databases` yürüterek HDInsight 4,0 kümesi içinden görünür olacaktır. HDInsight 4,0 kümelerinde sorgu yürütme hakkında bilgi için bkz. [HDInsight sürümleri arasında sorgu yürütme](#query-execution-across-hdinsight-versions) .

Ancak tablolardaki gerçek veriler, kümenin gerekli depolama hesaplarına erişimi olana kadar erişilebilir değildir. HDInsight 4,0 kümenizin eski HDInsight 3,6 kümeniz ile aynı verilere erişebildiğinizden emin olmak için aşağıdaki adımları izleyin:

1. Tablonuzun veya veritabanınızın Azure Depolama hesabını saptayın.

1. HDInsight 4,0 kümeniz zaten çalışıyorsa, Azure Depolama hesabını ambarı aracılığıyla kümeye ekleyin. HDInsight 4,0 kümesini henüz oluşturmadıysanız, Azure Storage hesabının birincil veya ikincil küme depolama hesabı olarak belirtildiğinden emin olun. HDInsight kümelerine depolama hesapları ekleme hakkında daha fazla bilgi için bkz. [HDInsight 'a ek depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md).

## <a name="deploy-new-hdinsight-40-and-connect-to-the-new-metastore"></a>Yeni HDInsight 4,0 dağıtma ve yeni meta veri deposu 'na bağlanma

Şema yükseltme işlemi tamamlandıktan sonra, yeni bir HDInsight 4,0 kümesi dağıtın ve yükseltilen meta veri deposunu bağlayın. Zaten 4,0 ' ı dağıttıysanız, ambarı 'ndan meta veri deposu 'na bağlanabilmeniz için bunu ayarlayın.

## <a name="run-schema-migration-script-from-hdinsight-40"></a>HDInsight 4,0 ' den şema geçiş betiği çalıştırma

Tablolar HDInsight 3,6 ve HDInsight 4,0 ' de farklı olarak değerlendirilir. Bu nedenle, farklı sürümlerin kümeleri için aynı tabloları paylaşamazsınız. HDInsight 3,6 ' i HDInsight 4,0 ile aynı anda kullanmak istiyorsanız, her sürüm için verilerin ayrı bir kopyasına sahip olmanız gerekir.

Hive iş yükünüz, ACID ve ACID olmayan tabloların bir karışımını içerebilir. HDInsight 3,6 (Hive 2) ve HDInsight 4,0 (Hive 3) üzerindeki Hive arasındaki bir temel fark, tablolar için ACID-uyumluluk ' dir. HDInsight 3,6 ' de, Hive ACID-Uyumluluk özelliğinin etkinleştirilmesi ek yapılandırma gerektirir, ancak HDInsight 4,0 tablolarında, varsayılan olarak ACID uyumludur. Geçişten önce gereken tek eylem, 3,6 kümesindeki ACID tablosuna göre büyük bir sıkıştırma çalıştırmak içindir. Hive görünümünden veya Beeline 'dan aşağıdaki sorguyu çalıştırın:

```sql
alter table myacidtable compact 'major';
```

HDInsight 3,6 ve HDInsight 4,0 ACID tabloları ACID değişimleri 'ı farklı şekilde anladığından bu sıkıştırma gereklidir. Düzenleme, tutarlılığı güvence altına alan temiz bir tablet uygular. [Hive geçiş belgelerinin](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-upgrade-major/content/prepare_hive_for_upgrade.html) 4. bölümü, HDInsight 3,6 ACID tablolarının toplu sıkıştırmasına yönelik kılavuz içerir.

Meta veri geçişini ve düzenleme adımlarını tamamladıktan sonra, gerçek ambarı geçirebilirsiniz. Hive ambarı geçişini tamamladıktan sonra, HDInsight 4,0 ambarı aşağıdaki özelliklere sahip olur:

|3,6 |4,0 |
|---|---|
|Dış tablolar|Dış tablolar|
|İşlem dışı yönetilen tablolar|Dış tablolar|
|İşlem tarafından yönetilen tablolar|Yönetilen tablolar|

Geçişi yürütmeden önce ambarınızın özelliklerini ayarlamanız gerekebilir. Örneğin, bazı tabloya üçüncü bir taraf (örneğin, HDInsight 3,6 kümesi) erişildiğini düşünüyorsanız, geçiş işlemi tamamlandıktan sonra bu tablo dış olmalıdır. HDInsight 4,0 ' de, tüm yönetilen tablolar işlem. Bu nedenle, HDInsight 4,0 ' deki yönetilen tablolara yalnızca HDInsight 4,0 kümeleri tarafından erişilmelidir.

Tablo özelliklerinizi doğru şekilde ayarladıktan sonra, SSH kabuğu 'nu kullanarak küme yayın düğümlerinden birindeki Hive ambarı geçiş aracını yürütün:

1. SSH kullanarak küme baş düğümüne 'a bağlanın. Yönergeler için bkz. [SSH kullanarak HDInsight 'A bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md)
1. `sudo su - hive` çalıştırarak Hive kullanıcısı olarak bir oturum açma kabuğu açın
1. `ls /usr/hdp`yürüterek veri platformu yığın sürümünü belirleme. Bu, sonraki komutta kullanmanız gereken bir sürüm dizesi görüntüler.
1. Kabuktan aşağıdaki komutu yürütün. `STACK_VERSION` önceki adımdaki sürüm dizesiyle değiştirin:

```bash
/usr/hdp/STACK_VERSION/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true -m automatic --modifyManagedTables
```

Geçiş aracı tamamlandıktan sonra, Hive ambarınız HDInsight 4,0 ' de hazırlanacaktır.

> [!Important]  
> HDInsight 4,0 ' deki yönetilen tablolara (3,6 ' den geçirilen tablolar dahil) HDInsight 3,6 kümeleri de dahil olmak üzere diğer hizmetler veya uygulamalar tarafından erişilmemelidir.

## <a name="secure-hive-across-hdinsight-versions"></a>HDInsight sürümleri arasında güvenli Hive

HDInsight 3,6 ' den itibaren HDInsight, HDInsight Kurumsal Güvenlik Paketi (ESP) kullanarak Azure Active Directory tümleştirilir. ESP, küme içindeki belirli kaynakların izinlerini yönetmek için Kerberos ve Apache Ranger kullanır. HDInsight 3,6 ' de Hive 'e karşı dağıtılan Ranger ilkeleri, aşağıdaki adımlarla HDInsight 4,0 ' e geçirilebilir:

1. HDInsight 3,6 kümenizdeki Ranger Service Manager paneline gidin.
2. **HIVE** adlı ilkeye gidin ve ilkeyi bir JSON dosyasına dışarı aktarın.
3. Yeni kümede, verilen ilke JSON öğesinde başvurulan tüm kullanıcıların var olduğundan emin olun. Bir kullanıcıya, ilke JSON ' de başvurulmazsa, ancak yeni kümede yoksa, kullanıcıyı yeni kümeye ekleyin veya ilkeden başvuruyu kaldırın.
4. HDInsight 4,0 kümenizdeki **Ranger Service Manager** paneline gidin.
5. **HIVE** adlı ilkeye gidin ve adım 2 ' den Ranger ilkesi JSON 'sini içeri aktarın.

## <a name="check-compatibility-and-modify-codes-as-needed-in-test-app"></a>Uyumluluğu denetle ve kodları test uygulaması 'nda gerektiği şekilde değiştir

Mevcut programlar ve sorgular gibi iş yüklerini geçirirken, lütfen sürüm notlarını ve değişiklikleri denetleyin ve değişiklikleri gerekli şekilde uygulayın. HDInsight 3,6 kümeniz paylaşılan bir Spark ve Hive meta veri deposu kullanıyorsa, [Hive ambarı bağlayıcısı kullanılarak ek yapılandırma](./apache-hive-warehouse-connector.md) gerekir.

## <a name="deploy-new-app-for-production"></a>Üretim için yeni uygulama dağıtma

Yeni kümeye geçmek için, örneğin yeni bir istemci uygulaması yükleyebilir ve yeni bir üretim ortamı olarak kullanabilir ya da mevcut istemci uygulamanızı yükseltebilir ve HDInsight 4,0 ' e geçebilirsiniz.

## <a name="switch-hdinsight-40-to-the-production"></a>HDInsight 4,0 ' i üretime değiştirme

Test sırasında meta veri deposu 'nda farklılıklar oluşturulmuşsa, geçiş yapmadan önce değişiklikleri güncelleştirmeniz gerekir. Bu durumda, dışarı aktarabilir & meta veri deposunu içeri aktarabilir ve sonra yeniden yükseltebilirsiniz.

## <a name="remove-the-old-production"></a>Eski üretimi kaldır

Yayının tamamlandığını ve tamamen çalıştığını doğruladıktan sonra 3,6 sürümünü ve önceki meta veri deposunu kaldırabilirsiniz. Lütfen ortamı silmeden önce her şeyin geçirildiğinden emin olun.

## <a name="query-execution-across-hdinsight-versions"></a>HDInsight sürümleri arasında sorgu yürütme

HDInsight 3,6 kümesi içinde Hive/LLAP sorgularını yürütmenin ve hata ayıklamanın iki yolu vardır. HiveCLI, bir komut satırı deneyimi sağlar ve tez görünümü/Hive görünümü GUI tabanlı bir iş akışı sağlar.

HDInsight 4,0 ' de HiveCLI, Beeline ile değiştirilmiştir. HiveCLI, Hiveserver 1 için bir Thrift istemcsahiptir ve Beeline, Hiveserver 2 ' ye erişim sağlayan bir JDBC istemcidedir. Diğer bir JDBC uyumlu veritabanı uç noktasına bağlanmak için Beeline de kullanılabilir. Beline, herhangi bir yükleme gerekmeden HDInsight 4,0 ' de kullanıma sunulmuştur.

HDInsight 3,6 ' de Hive sunucusu ile etkileşim için GUI istemcisi, ambarı Hive görünümüdür. HDInsight 4,0, ambarı görünümüyle birlikte gelmez. Müşterilerimizin, temel bir HDInsight hizmeti olmayan Data Analytics Studio 'Yu (DAS) kullanması için bir yol sağladık. DAS, HDInsight kümeleri ile kullanıma hazır değildir ve resmi olarak desteklenen bir paket değildir. Bununla birlikte, aşağıdaki gibi bir [betik eylemi](../hdinsight-hadoop-customize-cluster-linux.md) KULLANıLARAK kümeye das yüklenebilir:

|Özellik | Değer |
|---|---|
|Betik türü|-Özel|
|Adı|LARı|
|Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh`|
|Düğüm türleri|Baş|

5 ila 10 dakika bekleyin, sonra şu URL 'YI kullanarak Data Analytics Studio 'Yu başlatın: `https://CLUSTERNAME.azurehdinsight.net/das/`.

DAS yüklendikten sonra, sorgular görüntüleyicisinde çalıştırdığınız sorguları görmüyorsanız, aşağıdaki adımları uygulayın:

1. [Das yüklemesinde sorun giderme için bu kılavuzda](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html)açıklanan Hive, tez ve das için yapılandırma ayarlayın.
2. Aşağıdaki Azure depolama dizini yapılandırması ' nın sayfa Blobları olduğundan ve `fs.azure.page.blob.dirs`altında listelendiğinden emin olun:
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. Her iki headnode üzerinde de IV, Hive, tez ve DAS 'i yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight 4,0 duyurusu](../hdinsight-version-release.md)
* [HDInsight 4,0 derin bakış](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [Hive 3 ACID tabloları](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
