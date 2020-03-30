---
title: Azure HDInsight'ı kullanarak Sorun Giderme Kovanı
description: Apache Hive ve Azure HDInsight ile çalışma yla ilgili sık sorulan soruların yanıtlarını alın.
keywords: Azure HDInsight, Hive, SSS, sorun giderme kılavuzu, sık sorulan sorular
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.openlocfilehash: 02247adb9852a72b386feb2ef0924b0f1b3d6277
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895229"
---
# <a name="troubleshoot-apache-hive-by-using-azure-hdinsight"></a>Azure HDInsight kullanarak Apache Hive sorunlarını giderme

Apache Ambari'de Apache Hive yükleri ile çalışırken en önemli sorular ve onların çözümleri hakkında bilgi edinin.

## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Hive metastore'u nasıl dışa aktarıp başka bir kümeye aktarıyorum?

### <a name="resolution-steps"></a>Çözüm adımları

1. Güvenli Kabuk (SSH) istemcisi kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için [ek okumaya](#additional-reading-end)bakın.

2. Metastore'u dışa aktarmak istediğiniz HDInsight kümesinde aşağıdaki komutu çalıştırın:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

   Bu komut allatables.sql adlı bir dosya oluşturur.

3. Alltables.sql dosyasını yeni HDInsight kümesine kopyalayın ve ardından aşağıdaki komutu çalıştırın:

    ```apache
    hive -f alltables.sql
    ```

Çözüm adımlarında kod, yeni kümedeki veri yollarının eski kümedeki veri yolları ile aynı olduğunu varsayar. Veri yolları farklıysa, oluşturulan `alltables.sql` dosyayı değişiklikleri yansıtacak şekilde el ile değiştirebilirsiniz.

### <a name="additional-reading"></a>Ek okuma

- [SSH kullanarak HDInsight kümesine bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Bir kümedeki Hive günlüklerini nasıl bulurum?

### <a name="resolution-steps"></a>Çözüm adımları

1. SSH kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için **ek okumaya**bakın.

2. Hive istemci günlüklerini görüntülemek için aşağıdaki komutu kullanın:

   ```apache
   /tmp/<username>/hive.log
   ```

3. Hive metastore günlüklerini görüntülemek için aşağıdaki komutu kullanın:

   ```apache
   /var/log/hive/hivemetastore.log
   ```

4. Hive sunucu günlüklerini görüntülemek için aşağıdaki komutu kullanın:

   ```apache
   /var/log/hive/hiveserver2.log
   ```

### <a name="additional-reading"></a>Ek okuma

- [SSH kullanarak HDInsight kümesine bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a>Hive kabuğunu bir kümeüzerinde belirli yapılandırmalarla nasıl başlatırım?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hive kabuğunu başlattığınızda yapılandırma anahtar değeri çiftini belirtin. Daha fazla bilgi için [ek okumaya](#additional-reading-end)bakın.

   ```apache
   hive -hiveconf a=b
   ```

2. Hive kabuğundaki tüm etkili yapılandırmaları listelemek için aşağıdaki komutu kullanın:

   ```apache
   hive> set;
   ```

   Örneğin, konsolda hata ayıklama günlüğü etkinken Kovan kabuğunu başlatmak için aşağıdaki komutu kullanın:

   ```apache
   hive -hiveconf hive.root.logger=ALL,console
   ```

### <a name="additional-reading"></a>Ek okuma

- [Kovan yapılandırma özellikleri](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)

## <a name="how-do-i-analyze-apache-tez-dag-data-on-a-cluster-critical-path"></a><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Apache Tez DAG verilerini küme kritik bir yolda nasıl analiz edebilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. Apache Tez'in yönettiği asiklik grafiği (DAG) küme açısından kritik bir grafikte analiz etmek için, SSH kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için [ek okumaya](#additional-reading-end)bakın.

2. Komut isteminde aşağıdaki komutu çalıştırın:

   ```apache
   hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
   ```

3. Tez DAG'ı analiz etmek için kullanılabilecek diğer çözümleyicileri listelemek için aşağıdaki komutu kullanın:

   ```apache
   hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
   ```

   İlk bağımsız değişken olarak örnek bir program sağlamanız gerekir.

   Geçerli program adları şunlardır:
    - **ContainerReuseAnalyzer**: Bir DAG'da konteyner yeniden yazdırma kullanabilirsiniz
    - **CriticalPath**: Bir DAG kritik yolu bulun
    - **LocalityAnalyzer**: Yerellik ayrıntılarını BIR DAG'da yazdır
    - **ShuffleTimeAnalyzer**: Bir DAG'deki karıştırma zaman ayrıntılarını analiz edin
    - **SkewAnalyzer**: Bir DAG çarpık ayrıntıları analiz
    - **SlowNodeAnalyzer**: Bir DAG'da düğüm ayrıntılarını yazdır
    - **SlowTaskIdentifier**: Yavaş görev ayrıntılarını BIR DAG'da yazdırma
    - **SlowestVertexAnalyzer**: Bir DAG'de en yavaş tepe noktası ayrıntılarını yazdırın
    - **SpillAnalyzer**: Bir DAG yazdırma kdökme ayrıntıları
    - **TaskConcurrencyAnalyzer**: Görev eşzamanlılık ayrıntılarını bir DAG'ye yazdırın
    - **VertexLevelCriticalPathAnalyzer**: Bir DAG vertex düzeyinde kritik yolu bulun

### <a name="additional-reading"></a>Ek okuma

- [SSH kullanarak HDInsight kümesine bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Tez DAG verilerini bir kümeden nasıl indirebilirim?

#### <a name="resolution-steps"></a>Çözüm adımları

Tez DAG verilerini toplamanın iki yolu vardır:

- Komut satırından:

    SSH kullanarak HDInsight kümesine bağlanın. Komut isteminde aşağıdaki komutu çalıştırın:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId>
  ```

- Ambari Tez görünümünü kullanın:

  1. Ambari'ye git.
  2. Tez görünümüne gidin (sağ üst köşedeki kutucuk simgesinin altında).
  3. Görüntülemek istediğiniz DAG'yi seçin.
  4. **Veri İndir'i**seçin.

### <a name="additional-reading"></a><a name="additional-reading-end"></a>Ek okuma

[SSH kullanarak HDInsight kümesine bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

- [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
