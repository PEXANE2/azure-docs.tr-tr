---
title: Azure HDInsight 'ta bir Hive bellek yetersiz hatası giderme
description: HDInsight 'ta bir Hive bellek yetersiz hatası giderme. Müşteri senaryosu birçok büyük tablo üzerinde bir sorgudur.
keywords: yetersiz bellek hatası, OOM, Hive ayarları
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: hdinsightactive
ms.date: 11/28/2019
ms.openlocfilehash: add55c29bb93d8dce9ad69bd9850a1db02ea5afe
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74687762"
---
# <a name="fix-an-apache-hive-out-of-memory-error-in-azure-hdinsight"></a>Azure HDInsight 'ta bellek yetersiz Apache Hive hatasını çözme

Hive bellek ayarlarını yapılandırarak büyük tabloları işlerken bellek Apache Hive yetersiz (OOM) hatasını nasıl düzelteceğinizi öğrenin.

## <a name="run-apache-hive-query-against-large-tables"></a>Büyük tablolar üzerinde Apache Hive sorgusu Çalıştır

Bir müşteri Hive sorgusu çalıştırdı:

```sql
SELECT
    COUNT (T1.COLUMN1) as DisplayColumn1,
    …
    …
    ….
FROM
    TABLE1 T1,
    TABLE2 T2,
    TABLE3 T3,
    TABLE5 T4,
    TABLE6 T5,
    TABLE7 T6
where (T1.KEY1 = T2.KEY1….
    …
    …
```

Bu sorgunun bazı nusları:

* T1, çok sayıda DIZE sütunu türüne sahip TABLE1, büyük bir tablonun diğer adıdır.
* Diğer tablolar büyük değildir ancak çok sayıda sütun vardır.
* Tüm tablolar birbirini birleştiriyorsanız, bazı durumlarda TABLE1 ve diğer sütunlarda birden çok sütun bulunur.

Hive sorgusunun tamamlanması, 24 dakikalık bir a3 HDInsight kümesinde tamamlanır. Müşteri aşağıdaki uyarı iletilerini tespit ettik:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Apache Tez yürütme altyapısını kullanarak. Aynı sorgu 15 dakika boyunca çalışır ve şu hatayı oluşturdu:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Daha büyük bir sanal makine (örneğin, D12) kullanılırken hata kalır.

## <a name="debug-the-out-of-memory-error"></a>Bellek yetersiz hatası ayıklama hatası

Destek ve mühendislik ekiplerimiz, bellek yetersiz hatası nedeniyle [Apache JIRA ' da açıklanan bilinen bir sorun](https://issues.apache.org/jira/browse/HIVE-8306)olduğunu tespit ediyor.

"Hive. Auto. Convert. JOIN. noconditionaltask = true olduğunda noconditionaltask. size denetliyoruz ve harita birleştirmesindeki tablo boyutlarının toplamı noconditionaltask değerinden küçükse, bu planın bir harita birleşimi üretecekse bu, hesaplamanın bu şekilde sürmesidir Giriş boyutlarının toplamı, küçük bir kenar boşluğu sorguları tarafından noconditionaltask boyutundan küçükse, bu durum farklı karma uygulama tarafından sunulan ek yükü göz önüne almak için OOM 'ye vuracaktır. "

Hive-site. xml dosyasındaki **Hive. Auto. Convert. JOIN. noconditionaltask** , **true**olarak ayarlandı:

```xml
<property>
    <name>hive.auto.convert.join.noconditionaltask</name>
    <value>true</value>
    <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
    </description>
</property>
```

Büyük olasılıkla Map JOIN, Java yığın alanının bellek yetersiz hatası nedeniyle oluşur. Tez yürütme altyapısı, aslında tez kapsayıcısına ait olan bir yığın alanı kullanıldığında, [HDInsight 'Ta Hadoop Yarn bellek ayarlarında](https://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)blog gönderisi bölümünde açıklandığı gibi. Tez kapsayıcı belleğini açıklayan aşağıdaki resme bakın.

![Tez kapsayıcı bellek diyagramı: Hive bellek yetersiz hatası](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Blog gönderisi önerdiğinde, aşağıdaki iki bellek ayarı yığın için kapsayıcı belleğini tanımlar: **Hive. tez. Container. size** ve **Hive. tez. Java. opts**. Deneyimimizden, yetersiz bellek özel durumu, kapsayıcı boyutu çok küçük olduğu anlamına gelmez. Java yığın boyutunun (Hive. tez. Java. opts) çok küçük olduğu anlamına gelir. Bellek yetersiz olduğunda **Hive. tez. Java. opts**'yi artırmayı deneyebilirsiniz. Gerekirse **Hive. tez. Container. size**öğesini artırmanız gerekebilir. **Java. opts** ayarı kapsayıcının %80 ' inden oluşmalıdır. **Boyut**.

> [!NOTE]  
> **Hive. tez. Java. opts** ayarı her zaman **Hive. tez. Container. SIZE**değerinden küçük olmalıdır.

Bir D12 makinesinde 28 GB bellek bulunduğundan, 10 GB 'lık bir kapsayıcı boyutu (10240 MB) kullanmaya ve Java 'ya %80 ' i atamaya karar verdik. opts:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Yeni ayarlarla sorgu, 10 dakikadan kısa bir süre içinde başarıyla çalıştırıldı.

## <a name="next-steps"></a>Sonraki adımlar

OOM hatası alma, kapsayıcı boyutu çok küçük olduğu anlamına gelmez. Bunun yerine, bellek ayarlarını yığın boyutu artar ve kapsayıcı bellek boyutunun en az %80 ' i olacak şekilde yapılandırmanız gerekir. Hive sorgularını iyileştirmek için bkz. [HDInsight 'ta Apache Hadoop için Apache Hive sorgularını iyileştirme](hdinsight-hadoop-optimize-hive-query.md).
