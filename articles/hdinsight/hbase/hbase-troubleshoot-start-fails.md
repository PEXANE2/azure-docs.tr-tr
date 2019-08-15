---
title: Azure HDInsight 'ta Apache HBase Master başlatılamadı
description: Apache HBase Master (HMaster) Azure HDInsight 'ta başlatılamıyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/06/2019
ms.openlocfilehash: 8368ebfca4cdd72c5c455a04e29b6c0cb44938ea
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935412"
---
# <a name="apache-hbase-master-hmaster-fails-to-start-in-azure-hdinsight"></a>Apache HBase Master (HMaster) Azure HDInsight 'ta başlatılamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="scenario-atomic-renaming-failure"></a>Senaryo: Atomik yeniden adlandırma hatası

### <a name="issue"></a>Sorun

Başlangıç işlemi sırasında beklenmeyen dosyalar tanımlandı.

### <a name="cause"></a>Nedeni

Başlangıç işlemi sırasında, HMaster verileri sıfırdan (. tmp) klasörden veri klasörüne taşıma gibi birçok başlatma adımı gerçekleştirir. HMaster Ayrıca, herhangi bir ölü bölge sunucusu olup olmadığını görmek için WALs (ön günlüklere yaz) klasörüne de bakar. Tüm bu durumlar sırasında, bu klasörlerde temel `list` bir komut olur. Herhangi bir zamanda, bu klasörlerin hiçbirinde beklenmeyen bir dosya görürse, bir özel durum oluşturur ve bu nedenle başlamaz.

### <a name="resolution"></a>Çözüm

Böyle bir durumda, hangi klasörün soruna neden olabileceğini görmek için çağrı yığınını denetleyin (örneğin, It sals klasörü veya. tmp klasörü). Ardından, Cloud Explorer aracılığıyla veya sorun dosyasını bulmak için, bir. Sorun dosyası genellikle bir `*-renamePending.json` dosyadır (IDB sürücüsünde atomik yeniden adlandırma işlemini uygulamak için kullanılan bir günlük dosyası). Bu uygulamadaki hatalar nedeniyle, bu tür dosyalar işlem kilitlenmesi durumunda bırakılabilir. Bu dosyayı Cloud Explorer aracılığıyla silmeyi zorla. Ayrıca, bu konumda doğası $ 'in geçici bir dosyası olabilir. Dosya Cloud Explorer ile ve yalnızca `ls` bir. Bu dosyayı silmek için,. `hdfs dfs -rm //\$\$\$.\$\$\$`

Sorun dosyası kaldırıldıktan sonra, HMaster hemen başlamalıdır.

---

## <a name="scenario-no-server-address-listed"></a>Senaryo: Sunucu adresi listelenmedi

### <a name="issue"></a>Sorun

HMaster günlüğü, "HBase 'de sunucu adresi yok: xxx bölgesinde listelenen sunucu yok" iletisiyle benzer bir hata iletisi gösterir.

### <a name="cause"></a>Nedeni

HMaster, HBase yeniden başlatıldıktan sonra başlatılamadı.

### <a name="resolution"></a>Çözüm

1. HBase kabuğu 'nda aşağıdaki komutları yürütün (gerçek değerleri uygun olarak değiştirin):

    ```
    scan 'hbase:meta'
    delete 'hbase:meta','hbase:backup <region name>','<column name>' 
    ```

1. HBase: Namespace tablosu, ad alanı tablosu taranırken aynı hata ile bildirilirken ad alanı girdisini silin.

1. Çalışan durumunda HBase 'i açmak için, etkin HMaster 'ı ambarı kullanıcı arabiriminden yeniden başlatın.

1. Tüm çevrimdışı tabloları görüntülemek için HBase kabuğu 'nda aşağıdaki komutu çalıştırın:

    ```
    hbase hbck -ignorePreCheckPermission -fixAssignments
    ```

---

## <a name="scenario-javaioioexception-timedout"></a>Senaryo: Java. IO. IOException: Zaman aşımına uğradı

### <a name="issue"></a>Sorun

HMaster, gibi `java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned`önemli özel durumla zaman aşımına uğrar.

### <a name="cause"></a>Nedeni

Zaman aşımı, HMaster ile bilinen bir sorundur. Genel küme başlangıç görevleri uzun zaman alabilir. Ad alanı tablosu henüz atanmamışsa HMaster kapanır. Uzun başlangıç görevleri, büyük miktarda temizlenen verilerin mevcut olduğu ve beş dakikalık bir zaman aşımı yeterli olmadığı durumlarda gerçekleşir.

### <a name="resolution"></a>Çözüm

1. Ambarı Kullanıcı arabirimine erişin, HBase-> configs sayfasına gidin, özel `hbase-site.xml` ' de aşağıdaki ayarı ekleyin:

    ```
    Key: hbase.master.namespace.init.timeout Value: 2400000  
    ```

1. Gerekli hizmetleri (genellikle HMaster ve belki de diğer HBase hizmetlerini) yeniden başlatın.

---

## <a name="scenario-frequent-regionserver-restarts"></a>Senaryo: Sık kullanılan regionserver yeniden başlatmaları

### <a name="issue"></a>Sorun

Düğümler düzenli aralıklarla yeniden başlatılır. Regionserver günlüklerinden şuna benzer girdileri görebilirsiniz:

```
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
```

### <a name="cause"></a>Nedeni

Uzun regionserver JVM GC duraklatma. Duraklatma, regionserver 'ın yanıt vermemesine ve ZK oturum zaman aşımı 40s içinde HMaster 'a sinyal gönderememesine neden olur. HMaster, regionserver 'ın ölü olduğunu düşünmeyecek ve regionserver 'ı durduracaktır ve yeniden başlatılır.

### <a name="resolution"></a>Çözüm

Yalnızca HBase-site ayarını `zookeeper.session.timeout` değil, Zookeeper oturum zaman aşımını değiştirin, ancak aynı zamanda Zookeeper Zoo. cfg ayarının `maxSessionTimeout` değiştirilmesi gerekir.

1. Ambarı Kullanıcı arabirimine erişin, **HBase-> configs-> ayarlar**' a gidin, zaman aşımları bölümünde Zookeeper oturum zaman aşımı değerini değiştirin.

1. Ambarı Kullanıcı arabirimine erişin, **Zookeeper-> configs-> özel** Zoo. cfg adresine gidin, aşağıdaki ayarı ekleyin/değiştirin. Değerin HBase `zookeeper.session.timeout`ile aynı olduğundan emin olun.

    ```
    Key: maxSessionTimeout Value: 120000  
    ```

1. Gerekli hizmetleri yeniden başlatın.

---

## <a name="scenario-log-splitting-failure"></a>Senaryo: Günlük bölme hatası

### <a name="issue"></a>Sorun

Hmaster, bir HBase kümesine dönelemedi.

### <a name="cause"></a>Nedeni

İkincil depolama hesabı için yanlış yapılandırılmış ve HBase ayarları.

### <a name="resolution"></a>Çözüm

HBase. rootdir: wasb://@.blob.core.windows.net/hbase ' i ayarlayın ve ambarı 'nda hizmetleri yeniden başlatın.

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
