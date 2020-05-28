---
title: Azure HDInsight 3,6 Hive iş yüklerini HDInsight 'a geçirme 4,0
description: HDInsight 3,6 ' de Apache Hive iş yüklerini HDInsight 4,0 ' ye geçirmeyi öğrenin.
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: 003ee13220e9e8aae252e1a976d579beac870052
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84015021"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Azure HDInsight 3,6 Hive iş yüklerini HDInsight 'a geçirme 4,0

Bu belgede HDInsight 3,6 ' de Apache Hive ve LLAP iş yüklerinin HDInsight 4,0 ' ye nasıl geçirileceği gösterilmektedir. HDInsight 4,0, gerçekleştirilmiş görünümler ve sorgu sonucu önbelleği gibi daha yeni Hive ve LLAP özellikleri sağlar. İş yüklerinizi HDInsight 4,0 ' e geçirdiğinizde, Hive 3 ' ün HDInsight 3,6 ' de kullanılamayan daha yeni özelliklerinden yararlanabilirsiniz.

Bu makalede aşağıdaki konular ele alınmaktadır:

* Hive meta verilerinin HDInsight 'a geçirilmesi 4,0
* ACID ve ACID olmayan tabloların güvenli geçişi
* HDInsight sürümleri arasında Hive güvenlik ilkelerinin korunması
* HDInsight 3,6 ' den HDInsight 4,0 ' den sorgu yürütme ve hata ayıklama

Hive 'nin bir avantajı, meta verileri bir dış veritabanına dışarı aktarma (Hive meta veri deposu olarak adlandırılır) özelliğidir. **Hive meta** veri tablosu, tablo depolama konumu, sütun adları ve tablo dizini bilgileri de dahil olmak üzere tablo istatistiklerini depolamaktan sorumludur. HDInsight 3,6 ve HDInsight 4,0 farklı meta veri deposu şemaları gerektirir ve tek bir meta veri deposu paylaşamaz. Hive meta veri deposu güvenli bir şekilde yükseltmek için önerilen yol, geçerli üretim ortamında orijinal yerine bir kopyasını yükseltmenizde. Bu belge, özgün ve yeni kümelerin aynı depolama hesabına erişmesini gerektirir. Bu nedenle, başka bir bölgeye veri geçişini kapsamaz.

## <a name="migrate-from-external-metastore"></a>Dış meta veri deposu 'ndan geçiş

### <a name="1-run-major-compaction-on-acid-tables-in-hdinsight-36"></a>1. HDInsight 'ta ACID tablolarında büyük sıkıştırmayı çalıştırın 3,6

HDInsight 3,6 ve HDInsight 4,0 ACID tabloları ACID değişimleri 'ı farklı şekilde anlayın. Geçişten önce gereken tek eylem, 3,6 kümesindeki her bir ACID tablosuna göre ' Ana ' sıkıştırmayı çalıştırmak içindir. Düzenleme hakkındaki ayrıntılar için [Hive dilinde el ile](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-AlterTable/Partition/Compact) bakın.

### <a name="2-copy-sql-database"></a>2. SQL veritabanı 'nı kopyalayın
Dış meta veri deposu 'nun yeni bir kopyasını oluşturun. Dış meta veri deposu kullanıyorsanız, meta veri geri yükleme 'nin bir kopyasını oluşturmanın güvenli ve kolay yöntemlerinden biri, SQL veritabanı geri yükleme işlevini kullanarak veritabanını farklı bir adla [geri yüklemektir](../../azure-sql/database/recovery-using-backups.md#point-in-time-restore) .  HDInsight kümesine dış meta veri deposu ekleme hakkında daha fazla bilgi edinmek için bkz. [Azure HDInsight 'ta dış meta veri depoları kullanma](../hdinsight-use-external-metadata-stores.md) .

### <a name="3-upgrade-metastore-schema"></a>3. meta veri deposu şemasını yükselt
Meta veri **kopyalama** işlemi tamamlandıktan sonra, yeni meta veri deposunu Hive 3 şemasına yükseltmek Için mevcut HDInsight 3,6 kümesindeki [komut dosyası eyleminde](../hdinsight-hadoop-customize-cluster-linux.md) bir şema yükseltme betiği çalıştırın. (Bu adım, yeni meta veri deposunu bir kümeye bağlanmasını gerektirmez.) Bu, veritabanının HDInsight 4,0 meta veri deposu olarak eklenmesini sağlar.

Aşağıdaki tablodaki değerleri daha fazla kullanın. `SQLSERVERNAME DATABASENAME USERNAME PASSWORD`Hive meta veri deposu **kopyası**için boşluklarla ayırarak uygun değerlerle değiştirin. SQL Server adını belirtirken ". database.windows.net" eklemeyin.

|Özellik | Değer |
|---|---|
|Betik türü|-Özel|
|Name|Hive yükseltmesi|
|Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/launch-schema-upgrade.sh`|
|Düğüm türleri|Head|
|Parametreler|SQLSERVERNAME DATABASENAME KULLANıCı ADı PAROLASı|

> [!Warning]  
> HDInsight 3,6 meta veri şemasını HDInsight 4,0 şemasına dönüştüren yükseltme, geri alınamaz.

Veritabanında aşağıdaki SQL sorgusunu çalıştırarak yükseltmeyi doğrulayabilirsiniz:

```sql
select * from dbo.version
```

### <a name="4-deploy-a-new-hdinsight-40-cluster"></a>4. yeni bir HDInsight 4,0 kümesi dağıtın

1. Yükseltilen meta veri deposunu yeni kümenin Hive meta veri deposu olarak belirtin.

1. Ancak tablolardaki gerçek veriler, kümenin gerekli depolama hesaplarına erişimi olana kadar erişilebilir değildir.
HDInsight 3,6 kümesindeki Hive tablolarının depolama hesaplarının, yeni HDInsight 4,0 kümesinin birincil veya ikincil depolama hesapları olarak belirtildiğinden emin olun.
HDInsight kümelerine depolama hesapları ekleme hakkında daha fazla bilgi için bkz. [HDInsight 'a ek depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md).

### <a name="5-complete-migration-with-a-post-upgrade-tool-in-hdinsight-40"></a>5. HDInsight 'ta yükseltme sonrası bir araçla geçişi tam olarak yapın 4,0

Yönetilen tablolar, HDInsight 4,0 ' de varsayılan olarak ACID uyumlu olmalıdır. Meta veri geçişini tamamladıktan sonra, daha önce ACID olmayan yönetilen tabloları HDInsight 4,0 kümesiyle uyumlu hale getirmek için yükseltme sonrası bir araç çalıştırın. Bu araç aşağıdaki dönüştürmeyi uygular:

|3,6 |4.0 |
|---|---|
|Dış tablolar|Dış tablolar|
|ACID olmayan yönetilen tablolar|' External. Table. temizlemeyi ' = ' true ' özelliğine sahip dış tablolar|
|ACID yönetilen tabloları|ACID yönetilen tabloları|

SSH kabuğu 'nu kullanarak HDInsight 4,0 kümesinden Hive yükseltme sonrası aracı 'nı yürütün:

1. SSH kullanarak küme baş düğümüne 'a bağlanın. Yönergeler için bkz. [SSH kullanarak HDInsight 'A bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md)
1. Şunu çalıştırarak bir oturum açma kabuğunu Hive kullanıcısı olarak açın`sudo su - hive`
1. Kabuktan aşağıdaki komutu yürütün.

    ```bash
    STACK_VERSION=$(hdp-select status hive-server2 | awk '{ print $3; }')
    /usr/hdp/$STACK_VERSION/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true -m automatic --modifyManagedTables
    ```

Araç tamamlandıktan sonra, Hive ambarınız HDInsight 4,0 için hazırlanacaktır.

## <a name="migrate-from-internal-metastore"></a>İç meta veri deposu 'ndan geçiş

HDInsight 3,6 kümeniz bir iç Hive meta veri deposu kullanıyorsa, meta veri deposu 'ndan nesne tanımlarını dışarı aktarmak için Hive sorguları üreten bir betiği çalıştırmak için aşağıdaki adımları izleyin.

HDInsight 3,6 ve 4,0 kümelerinin aynı depolama hesabını kullanması gerekir.

> [!NOTE]
>
> * ACID tablolarında, tablonun altındaki verilerin yeni bir kopyası oluşturulur.
>
> * Bu betik yalnızca Hive veritabanlarının, tablolarının ve bölümlerinin geçirilmesini destekler. Görünümler, UDF 'ler ve tablo kısıtlamaları gibi diğer meta veri nesnelerinin el ile kopyalanması beklenmektedir.
>
> * Bu komut dosyası tamamlandıktan sonra, eski kümenin betikte başvuruda bulunulan tablolara veya veritabanlarına erişmek için artık kullanılmayacak olduğu varsayılır.
>
> * Tüm yönetilen tablolar, HDInsight 4,0 ' de işlem yapılabilir hale gelir. İsteğe bağlı olarak, verileri ' External. Table. temizlemeyi ' = ' true ' özelliğine sahip bir dış tabloya aktararak tabloyu işlem dışı tutun. Örneğin,
>
>    ```SQL
>    create table tablename_backup like tablename;
>    insert overwrite table tablename_backup select * from tablename;
>    create external table tablename_tmp like tablename;
>    insert overwrite table tablename_tmp select * from tablename;
>    alter table tablename_tmp set tblproperties('external.table.purge'='true');
>    drop table tablename;
>    alter table tablename_tmp rename to tablename;
>    ```

1. [Secure Shell (SSH) istemcisi](../hdinsight-hadoop-linux-use-ssh-unix.md)kullanarak HDInsight 3,6 kümesine bağlanın.

1. Açık SSH oturumunda, **alltables. HQL**adlı bir dosya oluşturmak için aşağıdaki betik dosyasını indirin.

    ```bash
    wget https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/exporthive_hdi_3_6.sh
    chmod 755 exporthive_hdi_3_6.sh
    ```

    * Normal bir HDInsight kümesi için, ESP olmadan yalnızca yürütün `exporthive_hdi_3_6.sh` .

    * ESP içeren bir küme için, kinıt ve bağımsız değişkenleri Beeline ile değiştirin: Azure AD kullanıcısına yönelik kullanıcı ve etkı ALANıNı tam Hive izinleriyle tanımlayarak aşağıdakileri çalıştırın.

        ```bash
        USER="USER"  # replace USER
        DOMAIN="DOMAIN"  # replace DOMAIN
        DOMAIN_UPPER=$(printf "%s" "$DOMAIN" | awk '{ print toupper($0) }')
        kinit "$USER@$DOMAIN_UPPER"
        ```

        ```bash
        hn0=$(grep hn0- /etc/hosts | xargs | cut -d' ' -f4)
        BEE_CMD="beeline -u 'jdbc:hive2://$hn0:10001/default;principal=hive/_HOST@$DOMAIN_UPPER;auth-kerberos;transportMode=http' -n "$USER@$DOMAIN" --showHeader=false --silent=true --outputformat=tsv2 -e"
        ./exporthive_hdi_3_6.sh "$BEE_CMD"
        ```

1. SSH oturumundan çıkın. Sonra **alltables. HQL** 'i yerel olarak indirmek için bir SCP komutu girin.

    ```bash
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:alltables.hql c:/hdi
    ```

1. **Alltables. HQL** ' i *Yeni* HDInsight kümesine yükleyin.

    ```bash
    scp c:/hdi/alltables.hql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

1. Ardından, *Yeni* HDInsight 4,0 kümesine bağlanmak için SSH kullanın. Aşağıdaki kodu bir SSH oturumundan bu kümeye çalıştırın:

    ESP olmadan:

    ```bash
    beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" -f alltables.hql
    ```

    ESP ile:

    ```bash
    USER="USER"  # replace USER
    DOMAIN="DOMAIN"  # replace DOMAIN
    DOMAIN_UPPER=$(printf "%s" "$DOMAIN" | awk '{ print toupper($0) }')
    kinit "$USER@$DOMAIN_UPPER"
    ```

    ```bash
    hn0=$(grep hn0- /etc/hosts | xargs | cut -d' ' -f4)
    beeline -u "jdbc:hive2://$hn0:10001/default;principal=hive/_HOST@$DOMAIN_UPPER;auth-kerberos;transportMode=http" -n "$USER@$DOMAIN" -f alltables.hql
    ```

3,6 HDInsight 'tan ACID yönetilmeyen tabloları, HDInsight 4,0 ' deki ACID tarafından yönetilen tablolara dönüştürülemediğinden, dış meta veri geçişi için yükseltme sonrası aracı burada uygulanmaz.

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
|Name|LARı|
|Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh`|
|Düğüm türleri|Head|

10 ila 15 dakika bekleyin ve ardından bu URL 'YI kullanarak Data Analytics Studio 'Yu başlatın: `https://CLUSTERNAME.azurehdinsight.net/das/` .

DAS 'e erişmeden önce, bir ambarı Kullanıcı arabirimi ve/veya tüm ambarı bileşenlerinin yeniden başlatılması gerekebilir.

DAS yüklendikten sonra, sorgular görüntüleyicisinde çalıştırdığınız sorguları görmüyorsanız, aşağıdaki adımları uygulayın:

1. [Das yüklemesinde sorun giderme için bu kılavuzda](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html)açıklanan Hive, tez ve das için yapılandırma ayarlayın.
2. Aşağıdaki Azure depolama dizini yapılandırması 'nın sayfa Blobları olduğundan ve altında listelendiğinden emin olun `fs.azure.page.blob.dirs` :
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. Her iki headnode üzerinde de IV, Hive, tez ve DAS 'i yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight 4,0 duyurusu](../hdinsight-version-release.md)
* [HDInsight 4,0 derin bakış](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [Hive 3 ACID tabloları](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
