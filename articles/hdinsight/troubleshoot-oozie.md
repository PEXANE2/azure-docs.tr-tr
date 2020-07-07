---
title: Azure HDInsight 'ta Apache Oozie sorunlarını giderme
description: Azure HDInsight 'ta bazı Apache Oozie hatalarıyla ilgili sorunları giderin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.openlocfilehash: 18831832f82cdbc8cec69e368f006f7acd4836c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82205267"
---
# <a name="troubleshoot-apache-oozie-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache Oozie sorunlarını giderme

Apache Oozie Kullanıcı arabirimi ile Oozie günlüklerini görüntüleyebilirsiniz. Oozie Kullanıcı arabirimi, iş akışı tarafından başlatılan MapReduce görevlerine yönelik JobTracker günlüklerine bağlantılar da içerir. Sorun giderme deseninin olması gerekir:

1. İşi Oozie Web Kullanıcı arabiriminde görüntüleyin.

2. Belirli bir eylem için hata veya hata oluşursa, **hata iletisi** alanının hata hakkında daha fazla bilgi sağladığını görmek için eylemi seçin.

3. Varsa, eylem için JobTracker günlükleri gibi daha fazla ayrıntı görüntülemek için eylemden URL 'YI kullanın.

Aşağıdakiler, içinde karşılaşabileceğiniz belirli hatalar ve bunları nasıl çözebileceğini aşağıda bulabilirsiniz.

## <a name="ja009-cant-initialize-cluster"></a>JA009: küme başlatılamıyor

### <a name="issue"></a>Sorun

İş durumu **askıya alındı**olarak değişir. İşin ayrıntıları `RunHiveScript` durumu **START_MANUAL**olarak gösterir. Eylemi seçmek şu hata iletisini görüntüler:

    JA009: Cannot initialize Cluster. Please check your configuration for map

### <a name="cause"></a>Nedeni

**job.xml** dosyasında kullanılan Azure Blob depolama adresleri depolama kapsayıcısı veya depolama hesabı adı içermiyor. BLOB depolama adresi biçimi olmalıdır `wasbs://containername@storageaccountname.blob.core.windows.net` .

### <a name="resolution"></a>Çözüm

İşin kullandığı BLOB depolama adreslerini değiştirin.

---

## <a name="ja002-oozie-isnt-allowed-to-impersonate-ltusergt"></a>JA002: Oozie kullanıcının kimliğine bürünmesine izin verilmiyor &lt;&gt;

### <a name="issue"></a>Sorun

İş durumu **askıya alındı**olarak değişir. İşin ayrıntıları `RunHiveScript` durumu **START_MANUAL**olarak gösterir. Eylemi seçerseniz, aşağıdaki hata iletisini gösterir:

    JA002: User: oozie is not allowed to impersonate <USER>

### <a name="cause"></a>Nedeni

Geçerli izin ayarları, Oozie 'nin belirtilen kullanıcı hesabını taklit etmesine izin vermez.

### <a name="resolution"></a>Çözüm

Oozie gruptaki kullanıcıları taklit edebilir **`users`** . `groups USERNAME`Kullanıcı hesabının üyesi olduğu grupları görmek için öğesini kullanın. Kullanıcı grubun üyesi değilse **`users`** , kullanıcıyı gruba eklemek için aşağıdaki komutu kullanın:

    sudo adduser USERNAME users

> [!NOTE]  
> HDInsight 'ın Kullanıcı gruba eklendiğini tanıması birkaç dakika sürebilir.

---

## <a name="launcher-error-sqoop"></a>Başlatıcı hatası (Sqoop)

### <a name="issue"></a>Sorun

İş durumu **sonlandırıldı**olarak değişir. İşin ayrıntıları `RunSqoopExport` durumu **hata**olarak gösterir. Eylemi seçerseniz, aşağıdaki hata iletisini gösterir:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

### <a name="cause"></a>Nedeni

Sqoop, veritabanına erişmek için gereken veritabanı sürücüsünü yükleyemiyor.

### <a name="resolution"></a>Çözüm

Bir Oozie işinden Sqoop kullandığınızda, iş tarafından kullanılan workflow.xml gibi diğer kaynaklarla veritabanı sürücüsünü dahil etmeniz gerekir. Ayrıca, workflow.xml bölümünden veritabanı sürücüsünü içeren arşive başvurun `<sqoop>...</sqoop>` .

Örneğin, iş örneği [Hadoop Oozie iş akışlarını kullanması](hdinsight-use-oozie-linux-mac.md)için aşağıdaki adımları kullanın:

1. `mssql-jdbc-7.0.0.jre8.jar`Dosyayı **/Tutorials/useoozie** dizinine kopyalayın:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre8.jar /tutorials/useoozie/mssql-jdbc-7.0.0.jre8.jar
    ```

2. `workflow.xml`AŞAĞıDAKI XML 'i yukarıdaki yeni bir satıra eklemek için öğesini değiştirin `</sqoop>` :

    ```xml
    <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
    ```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
