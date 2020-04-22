---
title: Azure HDInsight'ta komut dosyası eylemlerini sorun giderme
description: Azure HDInsight'ta komut dosyası eylemleri için genel sorun giderme adımları.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/21/2020
ms.openlocfilehash: b1e6b674edc155e0aa6c88ad360eb59864eebee4
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81771982"
---
# <a name="troubleshoot-script-actions-in-azure-hdinsight"></a>Azure HDInsight'ta komut dosyası eylemlerini sorun giderme

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="viewing-logs"></a>Günlükleri görüntüleme

Komut dosyası eylemleri tarafından günlüğe kaydedilen bilgileri görüntülemek için Apache Ambari web Kullanıcı Arabirimi'ni kullanabilirsiniz. Komut dosyası küme oluşturma sırasında başarısız olursa, günlükleri varsayılan küme depolama hesabında. Bu bölümde, her iki seçeneği de kullanarak günlüklerin nasıl alınabildiğini zedelemverilmektedir.

### <a name="apache-ambari-web-ui"></a>Apache Ambari web UI

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. Sayfanın üst kısmındaki çubuktan **ops** girişini seçin. Liste Ambari aracılığıyla kümeüzerinde yapılan geçerli ve önceki işlemleri görüntüler.

    ![Ambari web UI bar ops seçilen](./media/troubleshoot-script-action/hdi-apache-ambari-nav.png)

1. **İşlemler** sütununda **\_özel leştirilmiş komut dosyası çalıştıran** girişleri bulun. Bu girişler komut dosyası eylemleri çalıştırıldığında oluşturulur.

    ![Apache Ambari komut dosyası eylem işlemleri](./media/troubleshoot-script-action/ambari-script-action.png)

    **STDOUT** ve **STDERR** çıktısını görüntülemek için **run\customscriptaction** girişini seçin ve bağlantıları inceleştirin. Bu çıktı, komut dosyası çalıştığında oluşturulur ve yararlı bilgilere sahip olabilir.

### <a name="default-storage-account"></a>Varsayılan depolama hesabı

Komut dosyası hatası nedeniyle küme oluşturma başarısız olursa, günlükleri küme depolama hesabında tutulur.

* Depolama günlükleri ' `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`den temin edilebilir.

    ![Komut dosyası eylem günlükleri](./media/troubleshoot-script-action/script-action-logs-in-storage.png)

    Bu dizin altında, günlükleri **headnode**için ayrı ayrı düzenlenir , **işçi düğümü**, ve **zookeeper düğüm**. Aşağıdaki örneklere bakın:

    * **Kafa düğümü**:`<ACTIVE-HEADNODE-NAME>.cloudapp.net`

    * **İşçi düğümü**:`<ACTIVE-WORKERNODE-NAME>.cloudapp.net`

    * **Zookeeper düğümü**:`<ACTIVE-ZOOKEEPERNODE-NAME>.cloudapp.net`

* İlgili ana bilgisayara ait tüm **stdout** ve **stderr** depolama hesabına yüklenir. Her komut dosyası eylemi için bir **çıktı vardır-\*.txt** ve **hatalar-\*.txt.** **Output-*.txt** dosyası, ana bilgisayarda çalıştırılabilen komut dosyasının URI'si hakkında bilgi içerir. Aşağıdaki metin bu bilgilere bir örnektir:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Aynı ada sahip bir komut dosyası eylem kümesini art arda oluşturabilirsiniz. Bu durumda, **DATE** klasör adını temel alan ilgili günlükleri ayırt edebilirsiniz. Örneğin, farklı tarihlerde oluşturulan bir küme, **mycluster**klasör yapısı aşağıdaki günlük girişlerine benzer görünür:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Aynı gün aynı ada sahip bir komut dosyası eylem kümesi oluşturursanız, ilgili günlük dosyalarını tanımlamak için benzersiz önekik kullanabilirsiniz.

* Gece yarısı, saat 12:00'ye yakın bir küme oluşturursanız, günlük dosyalarının iki gün boyunca yayLımı olabilir. Bu durumda, aynı küme için iki farklı tarih klasörü görürsünüz.

* Günlük dosyalarını varsayılan kapsayıcıya yüklemek, özellikle büyük kümeler için beş dakika kadar sürebilir. Bu nedenle, günlüklere erişmek istiyorsanız, komut dosyası eylemi başarısız olursa kümeyi hemen silmemeniz gerekir.

## <a name="ambari-watchdog"></a>Ambari bekçi köpeği

Linux tabanlı HDInsight kümenizde Ambari izleme örgütü hdinsightwatchdog'un parolasını değiştirmeyin. Parola değişikliği, HDInsight kümesinde yeni komut dosyası eylemleri çalıştırma özelliğini kırar.

## <a name="cant-import-name-blobservice"></a>BlobService adını içe aktaramıyorum

__Semptomlar.__ Komut dosyası eylemi başarısız olur. Ambari'deki işlemi görüntülediğinizde aşağıdaki hataya benzer metin görüntülenir:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Çünkü.__ Bu hata, HDInsight kümesine dahil olan Python Azure Depolama istemcisini yükseltirseniz oluşur. HDInsight, Azure Depolama istemcisi 0.20.0 bekliyor.

__Çözünürlük__. Bu hatayı gidermek için, her küme düğümüne `ssh`kullanarak el ile bağlanın. Doğru depolama istemcisi sürümünü yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
sudo pip install azure-storage==0.20.0
```

Kümeye SSH ile bağlanma hakkında bilgi için, [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="history-doesnt-show-the-scripts-used-during-cluster-creation"></a>Geçmiş küme oluşturma sırasında kullanılan komut dosyalarını göstermez

Kümeniz 15 Mart 2016'dan önce oluşturulduysa, komut dosyası eylem geçmişinde bir giriş göremeyebilirsiniz. Kümeyi yeniden boyutlandırmak, komut dosyalarının komut dosyası eylem geçmişinde görünmesine neden olur.

İki özel durum vardır:

* Kümeniz 1 Eylül 2015'te oluşturuldu. Bu tarih, komut dosyası eylemlerinin tanıtıldığı tarihtir. Bu tarihten önce oluşturulan herhangi bir küme küme oluşturma için komut dosyası eylemleri kullanmış olamaz.

* Küme oluşturma sırasında birden çok komut dosyası eylemi kullandınız. Veya birden çok komut dosyası veya aynı ad, aynı URI, ancak birden çok komut dosyası için farklı parametreler için aynı adı kullandınız. Bu gibi durumlarda, aşağıdaki hatayı alırsınız:

    ```
    No new script actions can be run on this cluster because of conflicting script names in existing scripts. Script names provided at cluster creation must be all unique. Existing scripts are run on resize.
    ```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.