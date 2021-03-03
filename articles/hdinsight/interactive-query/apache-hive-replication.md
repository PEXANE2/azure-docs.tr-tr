---
title: Azure HDInsight kümelerinde Apache Hive çoğaltma kullanma
description: Hive meta veri deposu ve Azure Data Lake Storage Gen 2 Data Lake ' i çoğaltmak için HDInsight kümelerinde Hive çoğaltmasını nasıl kullanacağınızı öğrenin.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/08/2020
ms.openlocfilehash: 2e57b7d8121ac0bd6fc9cb693750c41a0ef6a5bc
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101733066"
---
# <a name="how-to-use-apache-hive-replication-in-azure-hdinsight-clusters"></a>Azure HDInsight kümelerinde Apache Hive çoğaltma kullanma

Veritabanları ve ambarların bağlamında, çoğaltma, varlıkları bir ambardan diğerine çoğaltma işlemidir. Çoğaltma, bir veritabanının tamamına veya tablo ya da bölüm gibi daha küçük bir düzeye uygulanabilir. Amaç, temel varlık her değiştiğinde değişen bir çoğaltmadır. Apache Hive çoğaltma, olağanüstü durum kurtarmasına odaklanır ve tek yönlü birincil kopya çoğaltma sağlar. HDInsight kümelerinde, Hive çoğaltması, Hive meta veri deposu ve ilişkili temel alınan veri Gölü Azure Data Lake Storage 2. üzerinde tek bir çoğaltma yapmak için kullanılabilir.  

Hive çoğaltma, daha iyi işlevsellik sağlayan ve daha hızlı ve daha az kaynak kullanımı daha yeni sürümlere sahip yıllar üzerinde gelişmiştir. Bu makalede, hem HDInsight 3,6 hem de HDInsight 4,0 küme türlerinde desteklenen Hive çoğaltmasını (Replv2) tartıştık.

## <a name="advantages-of-replv2"></a>Replv2 avantajları

[Hive ReplicationV2](https://cwiki.apache.org/confluence/display/Hive/HiveReplicationv2Development) (Replv2 olarak da bilinir), Hive çoğaltmasının, Hive [içeri aktarma](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ImportExport)kullanan ilk sürümü için aşağıdaki avantajlara sahiptir:

- Olay tabanlı artımlı çoğaltma
- Noktadan noktaya çoğaltma  
- Azaltılmış bant genişliği gereksinimleri  
- Ara kopya sayısında azaltma  
- Çoğaltma durumu korunur
- Kısıtlanmış çoğaltma  
- Hub ve bağlı bileşen modeli desteği  
- ACID tabloları desteği (HDInsight 4,0 ' de)

## <a name="replication-phases"></a>Çoğaltma aşamaları

Hive olay tabanlı çoğaltma, birincil ve ikincil kümeler arasında yapılandırılır. Bu çoğaltma iki farklı aşamadan oluşur: önyükleme ve artımlı çalıştırmalar.

### <a name="bootstrapping"></a>Önyükleme

Önyükleme, veritabanlarının temel durumunu Birincilden ikinciye çoğaltmak üzere bir kez çalışacak şekilde tasarlanmıştır. Gerekirse, çoğaltmanın etkinleştirilmesi gereken hedeflenen veritabanına tabloların bir alt kümesini dahil etmek için, gerekirse önyükleyiciyi yapılandırabilirsiniz.

### <a name="incremental-runs"></a>Artımlı çalıştırmalar

Önyükleme sonrasında, artımlı çalıştırmalar birincil kümede otomatikleştirilir ve bu artımlı çalıştırmalar sırasında oluşturulan olaylar ikincil kümede yürütülür. İkincil küme birincil kümeyle birlikte olduğunda, ikincil, birincil olay ile tutarlı hale gelir.

## <a name="replication-commands"></a>Çoğaltma komutları

Hive, `DUMP` `LOAD` olayların akışını düzenlemek için,, ve-bir REPL komutları kümesi sunar `STATUS` . `DUMP`Komut, birincil kümede tüm DDL/DML olaylarının yerel bir günlüğünü oluşturur. Bu `LOAD` komut, geç kopyalama meta verilerini ve ayıklanan çoğaltma dökümü çıkışına kaydedilen verileri bir yaklaşım ve hedef kümede yürütülür. Komut, en son `STATUS` çoğaltma yükünün başarıyla çoğaltılacağı en son olay kimliğini sağlamak için hedef kümeden çalışır.

### <a name="set-replication-source"></a>Çoğaltma kaynağını ayarla

Çoğaltmaya başlamadan önce, çoğaltılacak veritabanının çoğaltma kaynağı olarak ayarlandığından emin olun. `DESC DATABASE EXTENDED <db_name>`Parametresinin ilke adıyla ayarlanmış olup olmadığını anlamak için komutunu kullanabilirsiniz `repl.source.for` .

İlke zamanlanırsa ve `repl.source.for` parametresi ayarlanmamışsa, önce bu parametreyi kullanarak ayarlamanız gerekir `ALTER DATABASE <db_name> SET DBPROPERTIES ('repl.source.for'='<policy_name>')` .

```sql
ALTER DATABASE tpcds_orc SET DBPROPERTIES ('repl.source.for'='replpolicy1') 
```

### <a name="dump-metadata-to-the-data-lake"></a>Veri Gölü meta verilerin dökümünü al

`REPL DUMP [database name]. => location / event_id`Komut, Azure Data Lake Storage 2. için ilgili meta verilerin dökümünü almak üzere önyükleme aşamasında kullanılır. , `event_id` İlgili meta verilerin Azure Data Lake Storage 2. içine konululacağı en az olayı belirtir. 
 
```sql
repl dump tpcds_orc; 
```
Örnek çıktı:

| dump_dir|last_repl_id
|-|-|
|/tmp/Hive/REPL/38896729-67d5-41b2-90dc-46eeed4c5dd0|2925|

### <a name="load-data-to-the-target-cluster"></a>Hedef kümeye veri yükleme

`REPL LOAD [database name] FROM [ location ] { WITH ( ‘key1’=‘value1’{, ‘key2’=‘value2’} ) }`Komut hem önyükleme hem de artımlı aşamaları için hedef kümeye veri yüklemek üzere kullanılır. `[database name]`Kaynak veya hedef kümede farklı bir adla aynı olabilir. , `[location]` Önceki komutun çıktısından konumu temsil eder `REPL DUMP` . Bu, hedef kümenin kaynak kümeyle iletişim kurabilmesi gerektiği anlamına gelir. `WITH`Yan tümce öncelikle hedef kümenin yeniden başlatılmasını engellemek için eklenmiştir ve çoğaltmaya izin verir.

```sql
repl load tpcds_orc from '/tmp/hive/repl/38896729-67d5-41b2-90dc-46eeed4c5dd0'; 
```

### <a name="output-the-last-replicated-event-id"></a>Son çoğaltılan olay KIMLIĞINI çıkış

`REPL STATUS [database name]`Komut hedef kümeler üzerinde yürütülür ve son çoğaltılan çıkış çıkışları `event_id` . Komut Ayrıca, kullanıcıların hedef kümesinin hangi durumuna çoğaltılabileceğini bilmesini sağlar. Bu komutun çıktısını, artımlı çoğaltma için sonraki komutu oluşturmak üzere kullanabilirsiniz `REPL DUMP` .

```sql
repl status tpcds_orc;
```

Örnek çıktı:

|last_repl_id|
|-|
|2925|

### <a name="dump-relevant-data-and-metadata-to-the-data-lake"></a>Veri Gölü ilgili verilerin ve meta verilerin dökümünü alın

`REPL DUMP [database name] FROM [event-id] { TO [event-id] } { LIMIT [number of events] }`Komut, Azure Data Lake Storage ilgili meta verilerin ve verilerin dökümünü almak için kullanılır. Bu komut artımlı aşamada kullanılır ve kaynak ambarında çalıştırılır. , `FROM [event-id]` Artımlı aşama için gereklidir ve değeri `event-id` `REPL STATUS [database name]` Hedef ambardaki komutu çalıştırılarak türetilebilir.

```sql
repl dump tpcds_orc from 2925;
```

Örnek çıktı:

|dump_dir|last_repl_id|
|-|-|
| /tmp/Hive/REPL/38896729-67d5-41b2-90dc-466466agadd0 | 2960|

## <a name="hive-replication-process"></a>Hive çoğaltma işlemi

Aşağıdaki adımlar, Hive çoğaltma sürecinde gerçekleşen sıralı olaylardır.

1. Çoğaltılacak tabloların belirli bir ilke için çoğaltma kaynağı olarak ayarlandığından emin olun.

1. Bu `REPL_DUMP` komut, veritabanı adı, olay kimliği aralığı ve Azure Data Lake Storage 2. depolama URL 'si gibi ilişkili kısıtlamalara sahip birincil kümeye verilir.

1. Sistem, izlenen tüm olayların dökümünü, meta veri deposunu en son bir ile serileştirir. Bu döküm, tarafından belirtilen URL 'deki birincil kümedeki Azure Data Lake Storage 2. depolama hesabında depolanır `REPL_DUMP` .  

1. Birincil küme, çoğaltma meta verilerini birincil kümenin Azure Data Lake Storage 2. depolamasına devam ettirir. Yol, ambarı 'nda Hive yapılandırma Kullanıcı arabiriminde yapılandırılabilir. İşlem, meta verilerin depolandığı yolu ve en son izlenen DML/DDL olayını sağlar.

1. `REPL_LOAD`Komut, ikincil kümeden verilir. Komutu 3. adımda yapılandırılan yolu gösterir.

1. İkincil küme, adım 3 ' te oluşturulan izlenen olaylar ile meta veri dosyasını okur. İkincil kümenin, izlenen olayların depolandığı birincil kümenin Azure Data Lake Storage 2. depolamasına ağ bağlantısı olduğundan emin olun `REPL_DUMP` .  

1. İkincil küme, dağıtılmış kopya ( `DistCP` ) işlem olarak çoğaltılır.

1. İkincil küme, verileri birincil kümenin depolama alanından kopyalar.  

1. İkincil kümedeki meta veri deposu güncellenir.  

1. Son izlenen olay KIMLIĞI birincil meta veri deposu 'nda depolanır.

Artımlı çoğaltma aynı süreci izler ve son çoğaltılan olay KIMLIĞINI giriş olarak gerektirir. Bu, son çoğaltma olayından bu yana artımlı bir kopyaya yol açar. Artımlı çoğaltmalar normalde, gerekli kurtarma noktası hedeflerini (RPO) elde etmek için önceden belirlenen sıklığıyla otomatikleştirilebilir.

:::image type="content" source="media/apache-hive-replication/hive-replication-diagram.png" alt-text="Hive çoğaltma diyagramı":::

## <a name="replication-patterns"></a>Çoğaltma desenleri  

Çoğaltma normalde, birincil ve ikincil arasında tek yönlü bir şekilde yapılandırılmıştır ve burada, birincil ve ikincil, okuma ve yazma istekleri. İkincil küme, yalnızca okuma isteklerini okur. Bir olağanüstü durum varsa ikincil üzerinde yazma işlemleri yapılır, ancak ters çoğaltmanın birincil olarak yeniden yapılandırılması gerekir.

:::image type="content" source="media/apache-hive-replication/replication-pattern.png" alt-text="Hive çoğaltma kalıbı":::

Birincil – Ikincil, hub ve bağlı bileşen ve geçiş dahil olmak üzere Hive çoğaltması için uygun olan çok sayıda desen vardır.

HDInsight etkin birincil – bekleme Ikincili, ortak bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) deseninin yanı sıra HiveReplicationV2, bu düzenin, sanal ağ eşleme ile bölgesel olarak ayrılmış HDInsight Hadoop kümeleri ile kullanılmasına yol açabilir. Her iki kümeye eşlenen ortak bir sanal makine, çoğaltma Otomasyonu betikleri barındırmak için kullanılabilir. Olası HDInsight BCDR desenleri hakkında daha fazla bilgi için bkz. [HDInsight iş sürekliliği belgeleri](../hdinsight-business-continuity.md).  

### <a name="hive-replication-with-enterprise-security-package"></a>Kurumsal Güvenlik Paketi ile Hive çoğaltması  

Hive çoğaltmasının Kurumsal Güvenlik Paketi ile HDInsight Hadoop kümelerinde planlanmasının yanı sıra, Ranger meta veri deposu ve Azure Active Directory Domain Services (AD DS) için çoğaltma mekanizmalarını çarpanlara katmazsınız.  

Birden çok bölgede Azure AD kiracısı başına birden fazla Azure AD DS çoğaltma kümesi oluşturmak için Azure AD DS çoğaltma kümeleri özelliğini kullanın. Her bir çoğaltma kümesinin ilgili bölgelerindeki HDInsight sanal ağları ile eşlenilmesi gerekir. Bu yapılandırmada yapılandırma, Kullanıcı kimliği ve kimlik bilgileri, gruplar, Grup İlkesi nesneleri, bilgisayar nesneleri ve diğer değişiklikler dahil olmak üzere Azure AD DS değişiklikleri, yönetilen etki alanındaki tüm çoğaltma kümelerine Azure AD DS çoğaltma kullanılarak uygulanır.
  
Ranger ilkeleri, Ranger Import-Export işlevselliği kullanılarak düzenli aralıklarla yedeklenip Birincilden ikinciye çoğaltılabilir. İkincil kümede uygulamak istediğiniz yetkilendirmeler düzeyine bağlı olarak, Ranger ilkelerinin tümünü veya bir alt kümesini çoğaltmayı tercih edebilirsiniz.  

## <a name="sample-code"></a>Örnek kod  

Aşağıdaki kod sırası, önyükleme ve artımlı çoğaltmanın adlı örnek bir tabloda nasıl uygulanmasının bir örneğini sağlar `tpcds_orc` .  

1. Bir çoğaltma ilkesinin kaynağı olarak tabloyu ayarlayın.

   ```sql
   ALTER DATABASE tpcds_orc SET DBPROPERTIES ('repl.source.   for'='replpolicy1');
   ```

1. Birincil kümede önyükleme dökümü.

   ```sql
   repl dump tpcds_orc with ('hive.repl.rootdir'='/tmpag/hiveag/replag'); 
   ```
   
   Örnek çıktı:
   
   |dump_dir|last_repl_id|
   |-|-|
   |/tmpag/hiveag/deg/675d1kir-2361-4cad-bcbf-8680d305a27a|2925|
 
1. İkincil kümede önyükleme yükü. 

   ```sql
   repl load tpcds_orc from '/tmpag/hiveag/replag 675d1bea-2361-4cad-bcbf-8680d305a27a'; 
   ```

1. `REPL`İkincil kümedeki durumu kontrol edin.

   ```sql
   repl status tpcds_orc; 
   ```
 
   |last_repl_id|
   |-|
   |2925|

1. Birincil kümede artımlı döküm.

   ```sql
   repl dump tpcds_orc from 2925 with ('hive.repl.rootdir'='/tmpag/hiveag/ replag');
   ```

   Örnek çıktı:
   
   |dump_dir | last_repl_id|
   |-|-|
   |/tmpag/hiveag/13-g/31177ff7-a40f-4f67-a6 3b64ebe3bb31|2960|

1. İkincil kümede artımlı yük.  

   ```sql
   repl load tpcds_orc from '/tmpag/hiveag/replag/31177ff7-a40f-4f67-a613-3b64ebe3bb31';
   ```

1. `REPL`İkincil kümedeki durumu denetleyin.

   ```sql
   repl status tpcds_orc;
   ```

   |last_repl_id|
   |-|
   |2960|

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede ele alınan öğeler hakkında daha fazla bilgi edinmek için bkz.:

- [Azure HDInsight iş sürekliliği](../hdinsight-business-continuity.md)
- [Azure HDInsight iş sürekliliği mimarileri](../hdinsight-business-continuity-architecture.md)
- [Azure HDInsight yüksek oranda kullanılabilir çözüm mimarisi örnek olay incelemesi](../hdinsight-high-availability-case-study.md)
- [Azure HDInsight 'ta Apache Hive ve HiveQL nedir?](../hadoop/hdinsight-use-hive.md)