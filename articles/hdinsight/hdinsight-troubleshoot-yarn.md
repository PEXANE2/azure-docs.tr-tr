---
title: Azure HDInsight'ta Sorun Giderme İPLIĞI
description: Apache Hadoop YARN ve Azure HDInsight ile çalışma yla ilgili sık sorulan soruların yanıtlarını alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.openlocfilehash: f0c7b966b9fa7580809d2df0f4d05a7146ca0fd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272207"
---
# <a name="troubleshoot-apache-hadoop-yarn-by-using-azure-hdinsight"></a>Azure HDInsight kullanarak Apache Hadoop YARN sorunlarını giderme

Apache Ambari'de Apache Hadoop İPLik yükleri ile çalışırken en önemli sorunlar ve bunların çözümleri hakkında bilgi edinin.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Kümeüzerinde yeni bir İplik kuyruğu nasıl oluşturulur?

### <a name="resolution-steps"></a>Çözüm adımları

Yeni bir İplik sırası oluşturmak ve ardından tüm kuyruklar arasında kapasite tahsisini dengelemek için Ambari'de aşağıdaki adımları kullanın.

Bu örnekte, varolan iki kuyruk **(varsayılan** ve **tutumluluk)** her ikisi de %50'den %25 kapasiteye değiştirilir ve bu da yeni sıraya (kıvılcım) %50 kapasite verir.

| Kuyruk | Kapasite | Maksimum kapasite |
| --- | --- | --- |
| default | %25 | %50 |
| thrftsvr | %25 | %50 |
| spark | %50 | %50 |

1. **Ambari Görünümler** simgesini seçin ve ardından ızgara deseni seçin. Ardından, **İplik Sıra Yöneticisi'ni**seçin.

    ![Apache Ambari pano İplik Kuyruk Yöneticisi](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-1.png)
2. **Varsayılan** sırayı seçin.

    ![Apache Ambari YARN varsayılan sıra seçin](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-2.png)
3. **Varsayılan** sıra için **kapasiteyi** %50'den %25'e değiştirin. **Tutumluluk** sırası için **kapasiteyi** %25'e değiştirin.

    ![Varsayılan ve tutumlu kuyruklar için kapasiteyi %25'e değiştirme](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-3.png)
4. Yeni bir kuyruk oluşturmak için **Sıra Ekle'yi**seçin.

    ![Apache Ambari İplik panosu Sıra Ekle](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-4.png)

5. Yeni sırayı adlandırın.

    ![Apache Ambari İplik pano adı Sıra](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-5.png)  

6. **Kapasite** değerlerini %50'de bırakın ve ardından **Eylemler** düğmesini seçin.

    ![Apache Ambari YARN eylem seçin](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-6.png)  
7. **Kuyrukları Kaydet ve Yenile'yi**seçin.

    ![Kuyrukları Kaydet ve Yenile'yi seçin](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-7.png)  

Bu değişiklikler, İplik Zamanlayıcı UI'de hemen görülebilir.

### <a name="additional-reading"></a>Ek okuma

- [Apache Hadoop İplik KapasiteTprogramcısı](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)

## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Bir kümeden İplik günlüklerini nasıl indirebilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. Güvenli Kabuk (SSH) istemcisi kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için [ek okumaya](#additional-reading-2)bakın.

1. Şu anda çalışan İpLik uygulamalarının tüm uygulama dislerini listelemek için aşağıdaki komutu çalıştırın:

    ```apache
    yarn top
    ```

    Kimlikler **APPLICATIONID** sütununda listelenir. APPLICATIONID sütunundan **APPLICATIONID** günlükleri indirebilirsiniz.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

1. Tüm uygulama ustaları için İplik kapsayıcı günlüklerini indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Bu komut amlogs.txt adlı bir günlük dosyası oluşturur.

1. Yalnızca en son uygulama yöneticisi için İplik kapsayıcı günlüklerini indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Bu komut, sonamlogs.txt adlı bir günlük dosyası oluşturur.

1. İlk iki uygulama yöneticisi için İplik kapsayıcı günlüklerini indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt
    ```

    Bu komut first2amlogs.txt adlı bir günlük dosyası oluşturur.

1. Tüm İplik kapsayıcı günlüklerini indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Bu komut, logs.txt adlı bir günlük dosyası oluşturur.

1. Belirli bir kapsayıcı için İplik kapsayıcı günlüğünü indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt
    ```

    Bu komut, containerlogs.txt adlı bir günlük dosyası oluşturur.

### <a name="additional-reading"></a><a name="additional-reading-2"></a>Ek okuma

- [SSH kullanarak HDInsight'a (Apache Hadoop) bağlanın](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop İplik kavramları ve uygulamaları](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html#Concepts_and_Flow)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

- [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
