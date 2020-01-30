---
title: Erişim Apache Hadoop YARN uygulama günlükleri-Azure HDInsight
description: Hem komut satırı hem de bir Web tarayıcısı kullanarak Linux tabanlı HDInsight (Apache Hadoop) kümesinde YARN uygulama günlüklerine erişmeyi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/23/2020
ms.openlocfilehash: 2a7d71c6d751d4a48ec93f020e657a4d43114cfc
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76764373"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-linux-based-hdinsight"></a>Linux tabanlı HDInsight 'ta YARN uygulama günlüklerine erişim Apache Hadoop

Azure HDInsight 'ta bir [Apache Hadoop](https://hadoop.apache.org/) kümesindeki [Apache Hadoop Yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) (henüz başka bir kaynak Negotiator) uygulamaları için günlüklere erişmeyi öğrenin.

## <a name="what-is-apache-yarn"></a>Apache YARN nedir?

YARN, kaynak yönetimini uygulama zamanlama/izlemeye ayırarak birden çok programlama modelini ([Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) ) destekler. YARN, küresel bir *ResourceManager* (RM), Worker-node *nodeyöneticileri* (NMS) ve uygulama başına *applicationmaster* (AMS) kullanır. Uygulama başına, uygulamanızı RM ile çalıştırmaya yönelik kaynaklar (CPU, bellek, disk, ağ) tarafından anlaşma yapılır. RM, *kapsayıcılar*olarak verilen bu kaynakları vermek Için NMS ile birlikte çalışmaktadır. Bu, kendisine atanan kapsayıcıların, RM tarafından ilerlemesini izlemekten sorumludur. Uygulama, uygulamanın yapısına bağlı olarak çok sayıda kapsayıcı gerektirebilir.

Her uygulama birden çok *uygulama denemesinden*oluşabilir. Bir uygulama başarısız olursa, yeni bir deneme olarak yeniden denenebilir. Her deneme bir kapsayıcıda çalışır. Bir kapsayıcı, bir YARN uygulaması tarafından gerçekleştirilen temel çalışma birimi için bağlam sağlar. Bir kapsayıcı bağlamı içinde gerçekleştirilen tüm işler, kapsayıcının ayrıldığı tek çalışan düğümünde gerçekleştirilir. Bkz. [Hadoop: Yarn uygulamaları yazma](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html)veya daha fazla başvuru için [Yarn Apache Hadoop](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) .

Daha fazla işleme verimini desteklemek üzere kümenizi ölçeklendirmek için, birkaç farklı dil kullanarak [Otomatik ölçeklendirmeyi](hdinsight-autoscale-clusters.md) veya [kümelerinizi el ile ölçeklendirdirebilirsiniz](hdinsight-scaling-best-practices.md#utilities-to-scale-clusters).

## <a name="yarn-timeline-server"></a>YARN zaman çizelgesi sunucusu

[Apache Hadoop YARN zaman çizelgesi sunucusu](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) , tamamlanan uygulamalarla ilgili genel bilgiler sağlar

YARN zaman çizelgesi sunucusu aşağıdaki veri türlerini içerir:

* Uygulama KIMLIĞI, bir uygulamanın benzersiz tanımlayıcısı
* Uygulamayı başlatan Kullanıcı
* Uygulamayı tamamlamaya yönelik denemelere ilişkin bilgiler
* Belirli bir uygulama denemesi tarafından kullanılan kapsayıcılar

## <a name="yarn-applications-and-logs"></a>YARN uygulamaları ve günlükleri

YARN, kaynak yönetimini uygulama zamanlama/izlemeye ayırarak birden çok programlama modelini ([Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) ) destekler. YARN, küresel bir *ResourceManager* (RM), Worker-node *nodeyöneticileri* (NMS) ve uygulama başına *applicationmaster* (AMS) kullanır. Uygulama başına, uygulamanızı RM ile çalıştırmaya yönelik kaynaklar (CPU, bellek, disk, ağ) tarafından anlaşma yapılır. RM, *kapsayıcılar*olarak verilen bu kaynakları vermek Için NMS ile birlikte çalışmaktadır. Bu, kendisine atanan kapsayıcıların, RM tarafından ilerlemesini izlemekten sorumludur. Uygulama, uygulamanın yapısına bağlı olarak çok sayıda kapsayıcı gerektirebilir.

Her uygulama birden çok *uygulama denemesinden*oluşabilir. Bir uygulama başarısız olursa, yeni bir deneme olarak yeniden denenebilir. Her deneme bir kapsayıcıda çalışır. Bir kapsayıcı, bir YARN uygulaması tarafından gerçekleştirilen temel çalışma birimi için bağlam sağlar. Bir kapsayıcı bağlamı içinde gerçekleştirilen tüm işler, kapsayıcının ayrıldığı tek çalışan düğümünde gerçekleştirilir. Daha fazla başvuru için [Apache Hadoop YARN kavramlarını](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html) inceleyin.

Uygulama günlükleri (ve ilişkili kapsayıcı günlükleri), sorunlu Hadoop uygulamalarında hata ayıklama açısından kritik öneme sahiptir. YARN, uygulama günlüklerinin toplanması, toplanması ve [günlük toplama](https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/) özelliği ile depolanması için iyi bir çerçeve sağlar. Günlük toplama özelliği, uygulama günlüklerine daha belirleyici bir şekilde erişmenizi sağlar. Bir çalışan düğümündeki tüm kapsayıcılar üzerinde günlükleri toplar ve bunları çalışan düğümü başına bir toplu günlük dosyası olarak depolar. Bir uygulama bittikten sonra günlük varsayılan dosya sisteminde depolanır. Uygulamanız yüzlerce veya binlerce kapsayıcı kullanabilir, ancak tek bir çalışan düğümünde çalıştırılan tüm kapsayıcılar için Günlükler her zaman tek bir dosyaya toplanır. Bu nedenle, uygulamanız tarafından kullanılan her çalışan düğümü için yalnızca 1 günlük bir oturum vardır. Günlük toplama, HDInsight kümeleri sürüm 3,0 ve üzerinde varsayılan olarak etkindir. Toplanan Günlükler, küme için varsayılan depolamada bulunur. Aşağıdaki yol, günlüklerin bir yolu olarak verilmiştir:

```
/app-logs/<user>/logs/<applicationId>
```

Yolda, uygulamayı başlatan kullanıcının adı `user`. `applicationId`, YARN RM tarafından bir uygulamaya atanan benzersiz tanıtıcıdır.

Toplanan Günlükler, bir [tfile](https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf)dosyasında yazıldığı gibi doğrudan okunamaz, kapsayıcı tarafından dizine alınmış [ikili biçimdedir](https://issues.apache.org/jira/browse/HADOOP-3315) . Bu günlükleri uygulamalar veya ilgilendiğiniz kapsayıcılar için düz metin olarak görüntülemek üzere YARN ResourceManager günlüklerini veya CLı araçlarını kullanın.

## <a name="yarn-logs-in-an-esp-cluster"></a>Bir ESP kümesindeki Yarn günlükleri

Ambarı 'nda özel `mapred-site` iki yapılandırma eklenmelidir.

1. Bir Web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net`' a gidin; burada `CLUSTERNAME` kümenizin adıdır.

1. Ambarı kullanıcı arabiriminden, **gelişmiş** > **özel mapred-site** > **MapReduce2** > **configs** ' a gidin.

1. Aşağıdaki özellik kümelerinden *birini* ekleyin:

    **1 ayarla**

    ```
    mapred.acls.enabled=true
    mapreduce.job.acl-view-job=*
    ```

    **2 ayarla**

    ```
    mapreduce.job.acl-view-job=<user1>,<user2>,<user3>
    ```

1. Değişiklikleri kaydedin ve etkilenen tüm hizmetleri yeniden başlatın.

## <a name="yarn-cli-tools"></a>YARN CLı araçları

1. Kümenize bağlanmak için [SSH komutunu](./hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Şu komutu kullanarak şu anda çalışan Yarn uygulamalarının tüm uygulama kimliklerini listeleyin:

    ```bash
    yarn top
    ```

    Günlüklerin indirileceği `APPLICATIONID` sütundan uygulama kimliği ' ni aklınızda edin.

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

1. Aşağıdaki komutlardan birini çalıştırarak, bu günlükleri düz metin olarak görüntüleyebilirsiniz:

    ```bash
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
    ```

    Bu komutları çalıştırırken &lt;ApplicationId >, &lt;Kullanıcı tarafından başlatılan-uygulama >, &lt;Containerıd > ve &lt;çalışan düğümü-adresi > bilgilerini belirtin.

### <a name="other-sample-commands"></a>Diğer örnek komutlar

1. Aşağıdaki komutla tüm uygulama yöneticileri için Yarn kapsayıcıları günlüklerini indirin. Bu işlem, metin biçiminde `amlogs.txt` adlı günlük dosyasını oluşturur.

    ```bash
    yarn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

1. Yarn kapsayıcı günlüklerini yalnızca en son uygulama ana için aşağıdaki komutla indirin:

    ```bash
    yarn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

1. Aşağıdaki komutla, ilk iki uygulama ana için YARN kapsayıcı günlüklerini indirin:

    ```bash
    yarn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt
    ```

1. Tüm Yarn kapsayıcı günlüklerini aşağıdaki komutla indirin:

    ```bash
    yarn logs -applicationId <application_id> > logs.txt
    ```

1. Aşağıdaki komutla belirli bir kapsayıcı için Yarn kapsayıcı günlüğünü indirin:

    ```bash
    yarn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt
    ```

## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager Kullanıcı arabirimi

YARN ResourceManager Kullanıcı arabirimi küme headnode üzerinde çalışır. Bu, ambarı Web Kullanıcı arabirimi üzerinden erişilir. YARN günlüklerini görüntülemek için aşağıdaki adımları kullanın:

1. Web tarayıcınızda `https://CLUSTERNAME.azurehdinsight.net`' a gidin. CLUSTERNAME değerini HDInsight kümenizin adıyla değiştirin.

2. Soldaki hizmetler listesinden **Yarn**' yi seçin.

    ![Apache ambarı Yarn hizmeti seçildi](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarn-service-selected.png)

3. **Hızlı bağlantılar** açılan listesinden küme baş düğümlerinden birini seçip **ResourceManager günlüğü**' nü seçin.

    ![Apache ambarı Yarn hızlı bağlantıları](./media/hdinsight-hadoop-access-yarn-app-logs-linux/hdi-yarn-quick-links.png)

    YARN günlüklerine bağlantıların bir listesini görürsünüz.
