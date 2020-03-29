---
title: Apache Hadoop İPLik uygulama günlüklerine erişin - Azure HDInsight
description: Komut satırını ve web tarayıcısını kullanarak Linux tabanlı BIR HDInsight (Apache Hadoop) kümesindeki YARN uygulama günlüklerine nasıl erişeceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/23/2020
ms.openlocfilehash: 2a7d71c6d751d4a48ec93f020e657a4d43114cfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76764373"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-linux-based-hdinsight"></a>Linux tabanlı HDInsight'ta Apache Hadoop İPLik uygulama günlüklerine erişin

Azure HDInsight'taki [Apache Hadoop](https://hadoop.apache.org/) kümesindeki [Apache Hadoop IPN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) (Henüz Başka bir Kaynak Arabulucusu) uygulamalarının günlüklerine nasıl erişeceğinizi öğrenin.

## <a name="what-is-apache-yarn"></a>Apache İPLik nedir?

YARN, kaynak yönetimini uygulama planlamasından/izlemeden ayırarak birden fazla programlama modelini[(Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) bunlardan biri olarak) destekler. YARN, genel bir *Kaynak Yöneticisi* (RM), çalışan başına düğüm *Düğüm Yöneticileri* (NM) ve uygulama başına *ApplicationMasters* (AM) kullanır. Uygulama başına, uygulamanızı RM ile çalıştırmak için kaynakları (CPU, bellek, disk, ağ) görüşür. *RM, kapsayıcı*olarak verilen bu kaynakları sağlamak için NM'lerle birlikte çalışır. , RM tarafından kendisine atanan kapsayıcıların ilerlemesini izlemekten sorumludur. Bir uygulama, uygulamanın yapısına bağlı olarak birçok kapsayıcı gerektirebilir.

Her uygulama birden çok *uygulama denemesinden*oluşabilir. Bir uygulama başarısız olursa, yeni bir girişim olarak yeniden denenebilir. Her deneme bir kapsayıcıda çalışır. Bir anlamda, bir konteyner bir İplik uygulaması tarafından gerçekleştirilen temel çalışma birimi için bağlam sağlar. Bir kapsayıcı bağlamında yapılan tüm işler, kapsayıcının tahsis edildiği tek işçi düğümünde gerçekleştirilir. Bkz. [Hadoop: Yazma İplik Uygulamaları](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html), veya [Apache Hadoop İplik](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) daha fazla referans için.

Kümenizi daha fazla işlem verime sini desteklemek için ölçeklendirmek için, birkaç farklı dil kullanarak [Otomatik Ölçeklendirme'yi](hdinsight-autoscale-clusters.md) veya [kümelerinizi el ile ölçeklendirebilirsiniz.](hdinsight-scaling-best-practices.md#utilities-to-scale-clusters)

## <a name="yarn-timeline-server"></a>İplik Zaman Çizelgesi Sunucusu

[Apache Hadoop İPLik Zaman Çizelgesi Sunucusu,](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) tamamlanan uygulamalar hakkında genel bilgi sağlar

YARN Timeline Server aşağıdaki veri türünü içerir:

* Uygulama kimliği, bir uygulamanın benzersiz bir tanımlayıcısı
* Uygulamayı başlatan kullanıcı
* Başvuruyu tamamlamak için yapılan girişimler hakkında bilgi
* Herhangi bir uygulama girişimi tarafından kullanılan kaplar

## <a name="yarn-applications-and-logs"></a>İplik uygulamaları ve günlükleri

YARN, kaynak yönetimini uygulama planlamasından/izlemeden ayırarak birden fazla programlama modelini[(Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) bunlardan biri olarak) destekler. YARN, genel bir *Kaynak Yöneticisi* (RM), çalışan başına düğüm *Düğüm Yöneticileri* (NM) ve uygulama başına *ApplicationMasters* (AM) kullanır. Uygulama başına, uygulamanızı RM ile çalıştırmak için kaynakları (CPU, bellek, disk, ağ) görüşür. *RM, kapsayıcı*olarak verilen bu kaynakları sağlamak için NM'lerle birlikte çalışır. , RM tarafından kendisine atanan kapsayıcıların ilerlemesini izlemekten sorumludur. Bir uygulama, uygulamanın yapısına bağlı olarak birçok kapsayıcı gerektirebilir.

Her uygulama birden çok *uygulama denemesinden*oluşabilir. Bir uygulama başarısız olursa, yeni bir girişim olarak yeniden denenebilir. Her deneme bir kapsayıcıda çalışır. Bir anlamda, bir konteyner bir İplik uygulaması tarafından gerçekleştirilen temel çalışma birimi için bağlam sağlar. Bir kapsayıcı bağlamında yapılan tüm işler, kapsayıcının tahsis edildiği tek işçi düğümünde gerçekleştirilir. Daha fazla başvuru için [Apache Hadoop İplik Kavramlarına](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html) bakın.

Uygulama günlükleri (ve ilişkili kapsayıcı günlükleri) sorunlu Hadoop uygulamalarında hata ayıklama da önemlidir. YARN, [Log Agregasyon](https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/) özelliği ile uygulama günlüklerinin toplanması, toplanması ve depolanması için güzel bir çerçeve sağlar. Günlük Toplama özelliği, uygulama günlüklerine erişimi daha belirleyici hale getirir. Bir alt düğümdeki tüm kapsayıcılar arasında günlükleri toplar ve bunları çalışan düğüm başına birtoplu günlük dosyası olarak depolar. Günlük, uygulama bittikten sonra varsayılan dosya sisteminde depolanır. Uygulamanız yüzlerce veya binlerce kapsayıcı kullanabilir, ancak tek bir alt düğümüzerinde çalışan tüm kapsayıcılar için günlükler her zaman tek bir dosyaya toplanır. Yani uygulamanız tarafından kullanılan işçi düğümü başına yalnızca 1 günlük var. Günlük Toplama, HDInsight kümeleri sürüm 3.0 ve üzeri sürümde varsayılan olarak etkinleştirilir. Toplanan günlükler küme için varsayılan depolama bulunur. Aşağıdaki yol günlükleri IÇIN HDFS yoludur:

```
/app-logs/<user>/logs/<applicationId>
```

Yolda, `user` uygulamayı başlatan kullanıcının adıdır. Yarn `applicationId` RM tarafından bir uygulamaya atanan benzersiz tanımlayıcıdır.

Toplanan günlükler, kapsayıcı tarafından dizine eklenmiş [ikili biçimde](https://issues.apache.org/jira/browse/HADOOP-3315) bir [TFile'da](https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf)yazıldığı için doğrudan okunamıyor. Bu günlükleri uygulamalar veya ilgi çekici kapsayıcılar için düz metin olarak görüntülemek için İPLik ResourceManager günlüklerini veya CLI araçlarını kullanın.

## <a name="yarn-logs-in-an-esp-cluster"></a>Bir ESP kümesinde iplik günlükleri

Ambari'deki özele `mapred-site` iki yapılandırma eklenmelidir.

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. Ambari UI itibaren, **MapReduce2** > **Configs** > **Gelişmiş** > Özel**mapred-site**gidin.

1. Aşağıdaki özellik *kümelerinden birini* ekleyin:

    **Set 1**

    ```
    mapred.acls.enabled=true
    mapreduce.job.acl-view-job=*
    ```

    **Set 2**

    ```
    mapreduce.job.acl-view-job=<user1>,<user2>,<user3>
    ```

1. Değişiklikleri kaydedin ve etkilenen tüm hizmetleri yeniden başlatın.

## <a name="yarn-cli-tools"></a>İplik CLI araçları

1. Kümenize bağlanmak için [ssh komutunu](./hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Şu anda çalışan İplik uygulamalarının tüm uygulama kimliklerini aşağıdaki komutla listeleyin:

    ```bash
    yarn top
    ```

    Günlükleri indirilecek `APPLICATIONID` sütundaki uygulama kimliğine dikkat edin.

    ```output
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

1. Aşağıdaki komutlardan birini çalıştırarak bu günlükleri düz metin olarak görüntüleyebilirsiniz:

    ```bash
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
    ```

    &lt;Bu komutları çalıştırırken applicationId>, &lt;kullanıcı-who-started-the-application>, &lt;containerId> ve &lt;işçi düğümü adresi> bilgileri belirtin.

### <a name="other-sample-commands"></a>Diğer örnek komutlar

1. Aşağıdaki komutu ile tüm uygulama ustaları için İplik konteyner günlükleri indirin. Bu, metin biçiminde `amlogs.txt` adlı günlük dosyasını oluşturur.

    ```bash
    yarn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

1. Aşağıdaki komutu ile sadece en son uygulama yöneticisi için İplik konteyner günlükleri indirin:

    ```bash
    yarn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

1. İlk iki uygulama yöneticisi için İplik konteyner günlüklerini aşağıdaki komutla indirin:

    ```bash
    yarn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt
    ```

1. Aşağıdaki komutile tüm İplik konteyner günlükleri indirin:

    ```bash
    yarn logs -applicationId <application_id> > logs.txt
    ```

1. Aşağıdaki komutu ile belirli bir konteyner için iplik konteyner günlüğü indirin:

    ```bash
    yarn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt
    ```

## <a name="yarn-resourcemanager-ui"></a>İplik Kaynakları Müdürü UI

İPLik Kaynak Yöneticisi UI küme başlığı üzerinde çalışır. Ambari web ui üzerinden erişilir. İplik günlüklerini görüntülemek için aşağıdaki adımları kullanın:

1. Web tarayıcınızda, '' ye `https://CLUSTERNAME.azurehdinsight.net`gidin. CLUSTERNAME değerini HDInsight kümenizin adıyla değiştirin.

2. Soldaki hizmetler listesinden **YARN'ı**seçin.

    ![Apache Ambari İplik hizmeti seçildi](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarn-service-selected.png)

3. Hızlı **Bağlantılar** açılır bölümünden küme kafa düğümlerinden birini seçin ve ardından **ResourceManager Log'u**seçin.

    ![Apache Ambari İplik hızlı bağlantılar](./media/hdinsight-hadoop-access-yarn-app-logs-linux/hdi-yarn-quick-links.png)

    İplik günlüklerine bağlantı listesi sunulur.
