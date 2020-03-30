---
title: Azure HDInsight'ta bellek hatasından bir Kovanı düzeltme
description: HDInsight'ta bellek hatasından bir Kovanı düzeltin. Müşteri senaryosu, birçok büyük tabloda yapılan bir sorgudur.
keywords: bellek hatası, OOM, Hive ayarları
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: hdinsightactive
ms.date: 11/28/2019
ms.openlocfilehash: add55c29bb93d8dce9ad69bd9850a1db02ea5afe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687762"
---
# <a name="fix-an-apache-hive-out-of-memory-error-in-azure-hdinsight"></a>Azure HDInsight'ta bellek hatasından bir Apache Hive'ı düzeltme

Hive bellek ayarlarını yapılandırarak büyük tabloları işlerken apache Hive hatasını bellek dışı (OOM) hatasıyla nasıl düzelteceğinizi öğrenin.

## <a name="run-apache-hive-query-against-large-tables"></a>Apache Hive sorgusunu büyük tablolara karşı çalıştırma

Bir müşteri Hive sorgusu çalıştırdın:

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

Bu sorgunun bazı nüansları:

* T1, string sütun türleri çok olan büyük bir tablo, TABLE1, bir diğer adıdır.
* Diğer tablolar o kadar büyük değildir, ancak çok sayıda sütunu vardır.
* Tüm tablolar, bazı durumlarda TABLO1 ve diğerlerinde birden çok sütuniçeren birbiriyle birleşiyor.

Kovan sorgusunun 24 düğüm A3 HDInsight kümesinde tamamlanması 26 dakika sürdü. Müşteri aşağıdaki uyarı mesajlarını fark etti:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Apache Tez yürütme motorini kullanarak. Aynı sorgu 15 dakika boyunca çalıştırılır ve ardından aşağıdaki hatayı ataçıkmıştır:

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

Hata daha büyük bir sanal makine (örneğin, D12) kullanırken kalır.

## <a name="debug-the-out-of-memory-error"></a>Bellek hatası hatasını hata ayıklama

Bizim destek ve mühendislik ekipleri birlikte bellek hatası dışında neden sorunlardan biri [apache JIRA açıklanan bilinen](https://issues.apache.org/jira/browse/HIVE-8306)bir sorun bulundu:

"Kovan.auto.convert.join.noconditionaltask = true biz noconditionaltask.size kontrol ve harita birleştirme tabloların boyutları nın toplamı noconditionaltask.size plan bir Harita birleştirme oluşturacak daha az ise, bu hesaplama almaz küçük bir kenar boşluğu sorguları tarafından noconditionaltask boyutundan daha küçük ise sonuç olarak farklı HashTable uygulaması tarafından tanıtılan genel yükü dikkate oom vuracaktır."

**Kovan.auto.convert.join.noconditionaltask** kovan-site.xml dosyasında **gerçek**olarak ayarlandı :

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

Bu harita birleştirme bellek hatası java Yığın Alanı nedeni olması muhtemeldir. [HDInsight'taki hadoop İplik bellek ayarlarıblog](https://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)yazısında açıklandığı gibi, Tez yürütme motoru kullanıldığında kullanılan yığın alanı aslında Tez konteynerine aittir. Tez kapsayıcı belleği açıklayan aşağıdaki resme bakın.

![Tez konteyner bellek diyagramı: Bellek hatası ndan hive](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Blog gönderisinden de anlaşılacağı gibi, aşağıdaki iki bellek ayarı yığın için kapsayıcı bellek tanımlar: **kovan.tez.container.size** ve **hive.tez.java.opts**. Deneyimlerimize göre, bellek dışı özel durum, kapsayıcı boyutunun çok küçük olduğu anlamına gelmez. Bu Java yığın boyutu (hive.tez.java.opts) çok küçük olduğu anlamına gelir. Yani bellek dışında gördüğünüzde, **kovan.tez.java.opts**artırmak için deneyebilirsiniz . Gerekirse **kovan.tez.container.size**artırmak zorunda kalabilirsiniz . **Java.opts** ayarı **konteyner.size**yaklaşık% 80 olmalıdır.

> [!NOTE]  
> Ayar **hive.tez.java.opts** her zaman **kovan.tez.container.size**daha küçük olmalıdır .

Bir D12 makinesi 28 GB belleğe sahip olduğundan, 10 GB (10240 MB) konteyner boyutu kullanmaya karar verdik ve java.opts'e %80 atamaya karar verdik:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Yeni ayarlarla, sorgu 10 dakikanın altında başarıyla çalıştırılabilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

OOM hatası almak, kapsayıcı boyutunun çok küçük olduğu anlamına gelmez. Bunun yerine, yığın boyutunun artırılaması ve kapsayıcı bellek boyutunun en az %80'i olacak şekilde bellek ayarlarını yapılandırmanız gerekir. Hive sorgularını optimize etmek [için HDInsight'ta Apache Hadoop için Apache Hive sorgularını optimize etme 'ye](hdinsight-hadoop-optimize-hive-query.md)bakın.
