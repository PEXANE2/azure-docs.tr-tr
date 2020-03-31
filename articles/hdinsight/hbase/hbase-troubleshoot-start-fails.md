---
title: Apache HBase Master Azure HDInsight'ta başlayamadı
description: Apache HBase Master (HMaster) Azure HDInsight'ta başlayamadı
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/14/2019
ms.openlocfilehash: 290b541d9b5e86616373d2e426241fca07e780ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887215"
---
# <a name="apache-hbase-master-hmaster-fails-to-start-in-azure-hdinsight"></a>Apache HBase Master (HMaster) Azure HDInsight'ta başlayamadı

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="scenario-atomic-renaming-failure"></a>Senaryo: Atomik yeniden adlandırma hatası

### <a name="issue"></a>Sorun

Başlatma işlemi sırasında tanımlanan beklenmeyen dosyalar.

### <a name="cause"></a>Nedeni

Başlatma işlemi sırasında HMaster, verileri sıfırdan (.tmp) klasörüne taşıma dahil olmak üzere birçok başlatma adımı gerçekleştirir. HMaster ayrıca yanıt vermeyen bölge sunucuları olup olmadığını görmek için önden yazma günlükleri (WAL) klasörüne de bakar.

HMaster, WAL klasörlerinde temel bir liste komutu yapar. Herhangi bir zamanda, HMaster bu klasörlerin herhangi birinde beklenmeyen bir dosya görürse, bir özel durum atar ve başlatılamıyor.

### <a name="resolution"></a>Çözüm

Arama yığınını denetleyin ve soruna hangi klasörün neden olabileceğini belirlemeye çalışın (örneğin, WAL klasörü veya .tmp klasörü olabilir). Ardından, Bulut Gezgini'nde veya HDFS komutlarını kullanarak sorun dosyasını bulmaya çalışın. Genellikle, bu `*-renamePending.json` bir dosyadır. (Dosya `*-renamePending.json` WASB sürücüsüatomik yeniden adlandırma işlemini uygulamak için kullanılan bir günlük dosyasıdır. Bu uygulamadaki hatalar nedeniyle, bu dosyalar işlem çökmelerinden sonra bırakılabilir ve benzeri.) Cloud Explorer'da veya HDFS komutlarını kullanarak bu dosyayı zorla silin.

Bazen, bu konumda ki gibi `$$$.$$$` bir şey adında geçici bir dosya da olabilir. Bu dosyayı görmek `ls` için HDFS komutunu kullanmanız gerekir; Dosyayı Cloud Explorer'da göremezsiniz. Bu dosyayı silmek için HDFS komutunu `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`kullanın.

Bu komutları çalıştırdıktan sonra, HMaster hemen başlamalıdır.

---

## <a name="scenario-no-server-address-listed"></a>Senaryo: Listede sunucu adresi yok

### <a name="issue"></a>Sorun

Tablonun `hbase: meta` çevrimiçi olmadığını belirten bir ileti görebilirsiniz. Running, `hbck` HMaster günlüklerinde iletiyi görebilirsiniz: `No server address listed in hbase: meta for region hbase: backup <region name>`. `hbase: meta table replicaId 0 is not found on any region.`  

### <a name="cause"></a>Nedeni

HMaster, HBase'i yeniden başlattınktan sonra başlatılamadı.

### <a name="resolution"></a>Çözüm

1. HBase kabuğunda aşağıdaki komutları girin (geçerli olan gerçek değerleri değiştirin):

    ```hbase
    scan 'hbase:meta'
    delete 'hbase:meta','hbase:backup <region name>','<column name>'
    ```

1. `hbase: namespace` Girişi silin. Bu giriş, `hbase: namespace` tablo tarandığında bildirilen aynı hata olabilir.

1. Aktif HMaster'ı Ambari UI'den yeniden başlatArak HBase'i çalıştırma durumunda niçin gündeme getirin.

1. HBase kabuğunda, tüm çevrimdışı tabloları getirmek için aşağıdaki komutu çalıştırın:

    ```hbase
    hbase hbck -ignorePreCheckPermission -fixAssignments
    ```

---

## <a name="scenario-javaioioexception-timedout"></a>Senaryo: java.io.IOException: Timedout

### <a name="issue"></a>Sorun

HMaster zaman ölümcül özel durum `java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned`benzer ile dışarı: .

### <a name="cause"></a>Nedeni

HMaster hizmetlerinizi yeniden başlattığınızda temizlenmeyen çok sayıda tablo nuz ve bölgeniz varsa bu sorunla karşılaşabilirsiniz. Zaman-zaman zaman HMaster ile bilinen bir kusurdur. Genel küme başlatma görevleri uzun sürebilir. Ad alanı tablosu henüz atanmamışsa HMaster kapanır. Uzun başlangıç görevleri, büyük miktarda temizlenmemiş veri bulunduğu ve beş dakikalık bir zaman anın yeterli olmadığı durumlarda gerçekleşir.

### <a name="resolution"></a>Çözüm

1. Apache Ambari UI itibaren, **HBase** > **Configs**gidin. Özel `hbase-site.xml` dosyaya aşağıdaki ayarı ekleyin:

    ```
    Key: hbase.master.namespace.init.timeout Value: 2400000  
    ```

1. Gerekli hizmetleri (HMaster ve muhtemelen diğer HBase hizmetleri) yeniden başlatın.

---

## <a name="scenario-frequent-region-server-restarts"></a>Senaryo: Sık bölge sunucusu yeniden başlatılır

### <a name="issue"></a>Sorun

Düğümler düzenli olarak yeniden başlatılır. Bölge sunucu günlüklerinden aşağıdakilere benzer girişler görebilirsiniz:

```
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
```

### <a name="cause"></a>Nedeni

Uzun `regionserver` JVM GC duraklama. Duraklama yanıt `regionserver` vermeyen ve zk oturum zaman dışarı 40s içinde HMaster kalp atışı göndermek mümkün değil neden olur. HMaster öldüğüne inanacak `regionserver` ve `regionserver` iptal edip yeniden başlatacak.

### <a name="resolution"></a>Çözüm

Zookeeper oturum zaman ayarı `hbase-site` değiştirin, sadece ayar `zookeeper.session.timeout` değil, aynı zamanda Zookeeper `zoo.cfg` ayarı `maxSessionTimeout` değiştirilmesi gerekir.

1. Ambari UI'ye erişin, Zaman Aşımları bölümünde **Kitab-> Configs -> Ayarları'na**gidin, Zookeeper Session Timeout değerini değiştirin.

1. Ambari Kullanıcı Bira'sına erişin, **Zookeeper -> Configs -> Custom'a** `zoo.cfg`gidin, aşağıdaki ayarı ekleyin/değiştirin. Değerin HBase `zookeeper.session.timeout`ile aynı olduğundan emin olun.

    ```
    Key: maxSessionTimeout Value: 120000  
    ```

1. Gerekli hizmetleri yeniden başlatın.

---

## <a name="scenario-log-splitting-failure"></a>Senaryo: Günlük bölme hatası

### <a name="issue"></a>Sorun

HMasters bir HBase kümesi üzerinde gelmek için başarısız oldu.

### <a name="cause"></a>Nedeni

İkincil bir depolama hesabı için yanlış yapılandırılmış HDFS ve HBase ayarları.

### <a name="resolution"></a>Çözüm

hbase.rootdir'ı wasb://@.blob.core.windows.net/hbase ayarlayın: ve Ambari'deki hizmetleri yeniden başlatın.

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
