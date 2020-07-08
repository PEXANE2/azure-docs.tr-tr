---
title: Azure HDInsight 'ta betik eylemleri sorunlarını giderme
description: Azure HDInsight 'ta betik eylemleri için genel sorun giderme adımları.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: e8585779a263f4ff5dbdd998bbf065c6a4e1acdf
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86079274"
---
# <a name="troubleshoot-script-actions-in-azure-hdinsight"></a>Azure HDInsight 'ta betik eylemleri sorunlarını giderme

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="viewing-logs"></a>Günlükleri görüntüleme

Betik eylemleri tarafından günlüğe kaydedilen bilgileri görüntülemek için Apache ambarı Web Kullanıcı arabirimini kullanabilirsiniz. Küme oluşturma sırasında betik başarısız olursa Günlükler varsayılan küme depolama hesabıdır. Bu bölümde, bu seçeneklerin her ikisi de kullanılarak günlüklerin nasıl alınacağını gösteren bilgiler sağlanmaktadır.

### <a name="apache-ambari-web-ui"></a>Apache ambarı Web Kullanıcı arabirimi

1. Bir Web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net` , `CLUSTERNAME` Kümenizin adı olan ' a gidin.

1. Sayfanın üst kısmındaki çubuktan **Ops** girişini seçin. Bir liste, ambarı aracılığıyla kümede yapılan geçerli ve önceki işlemleri görüntüler.

    ![Ops seçiliyken ambarı Web UI çubuğu](./media/troubleshoot-script-action/hdi-apache-ambari-nav.png)

1. **İşlemler** sütununda ** \_ customscriptaction çalıştıran** girişleri bulun. Bu girişler, betik eylemleri çalıştırıldığında oluşturulur.

    ![Apache ambarı betik eylem işlemleri](./media/troubleshoot-script-action/ambari-script-action.png)

    **Stdout** ve **stderr** çıkışını görüntülemek için **run\customscriptaction** girişini seçin ve bağlantılar aracılığıyla detaya gidin. Bu çıktı, komut dosyası çalıştırıldığında üretilir ve yararlı bilgiler içerebilir.

### <a name="default-storage-account"></a>Varsayılan depolama hesabı

Küme oluşturma bir betik hatası nedeniyle başarısız olursa Günlükler küme depolama hesabında tutulur.

* Depolama günlükleri ' nde kullanılabilir `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` .

    ![Betik eylemi günlükleri](./media/troubleshoot-script-action/script-action-logs-in-storage.png)

    Bu dizin altında Günlükler, **baş düğümüne**, **çalışan düğümü**ve **Zookeeper düğümü**için ayrı olarak düzenlenir. Aşağıdaki örneklere bakın:

    * **Headnode**:`<ACTIVE-HEADNODE-NAME>.cloudapp.net`

    * **Çalışan düğümü**:`<ACTIVE-WORKERNODE-NAME>.cloudapp.net`

    * **Zookeeper düğümü**:`<ACTIVE-ZOOKEEPERNODE-NAME>.cloudapp.net`

* Karşılık gelen konağın tüm **stdout** ve **stderr** depolama hesabına yüklenir. Her betik eylemi için bir **output- \* . txt** ve **Errors- \* . txt** vardır. **Output-*. txt** dosyası, konakta çalıştırılan betiğin URI 'si hakkında bilgiler içerir. Aşağıdaki metin bu bilgilere bir örnektir:

    ```output
    'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'
    ```

* Yinelenen olarak aynı ada sahip bir betik eylemi kümesi oluşturmanız mümkündür. Bu durumda, ilgili günlükleri **Tarih** klasörü adı temelinde ayırabilirsiniz. Örneğin, bir kümenin klasör yapısı, farklı tarihlerde oluşturulan **MyCluster**, aşağıdaki günlük girdilerine benzer şekilde görünür:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Aynı günde aynı ada sahip bir betik eylemi kümesi oluşturursanız, ilgili günlük dosyalarını tanımlamak için benzersiz öneki kullanabilirsiniz.

* Gece yarısı 12:00 ' de bir küme oluşturursanız, günlük dosyalarının iki gün boyunca yayılması olasıdır. Bu durumda, aynı küme için iki farklı tarih klasörü görürsünüz.

* Günlük dosyalarını varsayılan kapsayıcıya yüklemek, özellikle büyük kümeler için beş dakikaya kadar sürebilir. Bu nedenle, günlüklere erişmek istiyorsanız, bir betik eylemi başarısız olursa kümeyi hemen silmelisiniz.

## <a name="ambari-watchdog"></a>Ambarı izleme

Hdinsightwatchdog, Linux tabanlı HDInsight kümenizdeki ambarı izleme, için parolayı değiştirmeyin. Parola değişikliği, HDInsight kümesinde yeni betik eylemleri çalıştırma yeteneğini keser.

## <a name="cant-import-name-blobservice"></a>BlobService adı içeri aktarılamıyor

__Belirtiler__. Betik eylemi başarısız olur. Aşağıdaki hata gibi metinler, ambarı 'nda işlemi görüntülediğinizde görüntülenir:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Neden__. Bu hata, HDInsight kümesine dahil edilen Python Azure depolama istemcisini yükseltirseniz oluşur. HDInsight, Azure Storage Client 0.20.0 gerektirir.

__Çözümleme__. Bu hatayı çözmek için kullanarak her bir küme düğümüne el ile bağlanın `ssh` . Doğru depolama istemci sürümünü yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
sudo pip install azure-storage==0.20.0
```

SSH ile kümeye bağlanma hakkında daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'a (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="history-doesnt-show-the-scripts-used-during-cluster-creation"></a>Geçmiş, küme oluşturma sırasında kullanılan betikleri göstermiyor

Kümeniz 15 Mart 2016 ' den önce oluşturulduysa, betik eylemi geçmişinde bir giriş görmeyebilirsiniz. Kümenin yeniden boyutlandırılması, betiklerin betik eylemi geçmişinde görünmesine neden olur.

İki özel durum vardır:

* Kümeniz 1 Eylül 2015 ' den önce oluşturulmuştur. Bu tarih, betik eylemlerinin tanıtılmasından yapılır. Bu tarihten önce oluşturulan herhangi bir küme, küme oluşturma için betik eylemlerini kullanamadı.

* Küme oluşturma sırasında birden çok betik eylemi kullandınız. Ya da birden çok komut dosyası için aynı adı, aynı URI 'yi, ancak birden çok komut dosyası için farklı parametreleri kullandınız. Bu durumlarda, şu hatayı alırsınız:

    ```
    No new script actions can be run on this cluster because of conflicting script names in existing scripts. Script names provided at cluster creation must be all unique. Existing scripts are run on resize.
    ```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
