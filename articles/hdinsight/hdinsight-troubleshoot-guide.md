---
title: Azure HDInsight sorun giderme kılavuzları
description: Azure HDInsight'ı kullanarak Apache Hadoop iş yüklerini sorun giderin. Adım adım belgeler, Apache Hive, Apache Spark, Apache YARN, Apache HBase, HDFS ve Apache Storm ile sık karşılaşılan sorunları çözmek için HDInsight'ı nasıl kullanacağınızı gösterir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/14/2019
ms.openlocfilehash: 4460c24bb742bff72af47954363b1051f5d9b43d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895282"
---
# <a name="troubleshoot-by-using-azure-hdinsight"></a>Azure HDInsight'ı kullanarak sorun giderme

| Apache iş yükü | En çok sorulan sorular |
|---|---|
|![hdinsight apache HBase simgesi](./media/hdinsight-troubleshoot-guide/hdinsight-apache-hbase.png)<br>[Apache HBase sorunlarını giderme](hbase/apache-troubleshoot-hbase.md)|<br>[Atanmamış bölgeler](hbase/hbase-troubleshoot-unassigned-regions.md#scenario-unassigned-regions)<br><br>[Azure HDInsight'ta 'hbase hbck' komutuyla zaman aşımları](hbase/hbase-troubleshoot-timeouts-hbase-hbck.md)<br><br>[Azure HDInsight'ta Apache Phoenix bağlantı sorunları](hbase/hbase-troubleshoot-phoenix-connectivity.md)<br><br>[Ana sunucunun başlatılamamasına ne neden olur?](hbase/hbase-troubleshoot-start-fails.md)<br><br>[BindException - Kullanımda olan adres](hbase/hbase-troubleshoot-bindexception-address-use.md)|
|![hdinsight apache hdfs simgesi simgesi](./media/hdinsight-troubleshoot-guide/hdinsight-apache-hdfs.png)<br>[Sorun Giderme Apache Hadoop HDFS](hdinsight-troubleshoot-hdfs.md)|<br>[Bir kümenin içinden yerel bir HDFS'ye nasıl erişebilirim?](hdinsight-troubleshoot-hdfs.md#how-do-i-access-local-hdfs-from-inside-a-cluster)<br><br>[Azure HDInsight kümesinde güvenli modda sıkışmış yerel HDFS](hadoop/hdinsight-hdfs-troubleshoot-safe-mode.md)|
|![hdinsight apache Hive simgesi simgesi](./media/hdinsight-troubleshoot-guide/hdinsight-apache-hive.png)<br>[Sorun Giderme Apache Hive](hdinsight-troubleshoot-hive.md)|<br>[Hive metastore'u nasıl dışa aktarıp başka bir kümeye aktarıyorum?](hdinsight-troubleshoot-hive.md#how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster)<br><br>[Bir kümedeki Apache Hive günlüklerini nasıl bulabiliyorum?](hdinsight-troubleshoot-hive.md#how-do-i-locate-hive-logs-on-a-cluster)<br><br>[Apache Hive kabuğunu bir kümeüzerinde belirli yapılandırmalarla nasıl başlatırım?](hdinsight-troubleshoot-hive.md#how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster)<br><br>[Apache Tez DAG verilerini küme kritik bir yolda nasıl analiz edebilirim?](hdinsight-troubleshoot-hive.md#how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path)<br><br>[Apache Tez DAG verilerini bir kümeden nasıl indirebilirim?](hdinsight-troubleshoot-hive.md#how-do-i-download-tez-dag-data-from-a-cluster)|
|![hdinsight apache Kıvılcım simgesi simgesi](./media/hdinsight-troubleshoot-guide/hdinsight-apache-spark.png)<br>[Sorun Giderme Apache Spark](hdinsight-troubleshoot-SPARK.md)|<br>[Kümeler üzerinde Apache Ambari kullanarak bir Apache Spark uygulamasını nasıl yapılandırabilirim?](spark/apache-troubleshoot-spark.md#how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters)<br><br>[Kümeler üzerinde Jupyter notebook kullanarak bir Apache Spark uygulamasını nasıl yapılandırabilirim?](spark/apache-troubleshoot-spark.md#how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters)<br><br>[Kümeler üzerinde Apache Livy kullanarak bir Apache Spark uygulamasını nasıl yapılandırabilirim?](spark/apache-troubleshoot-spark.md#how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters)<br><br>[Kümeler üzerinde spark-submit kullanarak bir Apache Spark uygulamasını nasıl yapılandırabilirim?](spark/apache-troubleshoot-spark.md#how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters)<br><br>[IntelliJ kullanarak bir Apache Spark uygulamasını nasıl yapılandırıyorum?](spark/apache-spark-intellij-tool-plugin.md)<br><br>[Eclipse kullanarak bir Apache Spark uygulamasını nasıl yapılandırıyorum?](spark/apache-spark-eclipse-tool-plugin.md)<br><br>[VsCode kullanarak bir Apache Spark uygulamasını nasıl yapılandırıyorum?](hdinsight-for-vscode.md)<br><br>[Apache Spark için OutOfMemoryError özel durumu](spark/apache-spark-troubleshoot-outofmemory.md#scenario-outofmemoryerror-exception-for-apache-spark)|
|![hdinsight apache Storm simgesi simgesi](./media/hdinsight-troubleshoot-guide/hdinsight-apache-storm.png)<br>[Sorun Giderme Apaçi Fırtınası](hdinsight-troubleshoot-STORM.md)|<br>[Bir kümedeki Apache Storm UI'ye nasıl erişebilirim?](storm/apache-troubleshoot-storm.md#how-do-i-access-the-storm-ui-on-a-cluster)<br><br>[Apache Storm olay merkezi spout denetim noktası bilgilerini bir topolojiden diğerine nasıl aktarabilirim?](storm/apache-troubleshoot-storm.md#how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another)<br><br>[Bir kümedeki Fırtına ikililerini nasıl bulurum?](storm/apache-troubleshoot-storm.md#how-do-i-locate-storm-binaries-on-a-cluster)<br><br>[Bir Fırtına kümesinin dağıtım topolojisini nasıl belirleyebilirim?](storm/apache-troubleshoot-storm.md#how-do-i-determine-the-deployment-topology-of-a-storm-cluster)<br><br>[Apache Storm etkinlik merkezi nin geliştirilmesi için ikili lerini nasıl bulurum?](storm/apache-troubleshoot-storm.md#how-do-i-locate-storm-event-hub-spout-binaries-for-development)|
|![hdinsight apache YARN simgesi simgesi](./media/hdinsight-troubleshoot-guide/hdinsight-apache-yarn.png)<br>[Sorun Giderme Apache Hadoop İplik](hdinsight-troubleshoot-YARN.md)|<br>[Bir kümeüzerinde yeni bir Apache Hadoop İplik kuyruğunasıl oluşturulur?](hdinsight-troubleshoot-yarn.md#how-do-i-create-a-new-yarn-queue-on-a-cluster)<br><br>[Apache Hadoop İplik günlüklerini bir kümeden nasıl indirebilirim?](hdinsight-troubleshoot-yarn.md#how-do-i-download-yarn-logs-from-a-cluster)|

## <a name="hdinsight-troubleshooting-resources"></a>HDInsight sorun giderme kaynakları

| Hakkında bilgi için | Bu makalelere göz atın |
| --- | --- |
| LINUX ve optimizasyon üzerine HDInsight | - [LINUX'ta HDInsight kullanma hakkında bilgi](hdinsight-hadoop-linux-information.md)<br>- [Apache Hadoop bellek ve performans sorun giderme](hdinsight-hadoop-stack-trace-error-messages.md)<br>- [Apache Hive sorgu performansı](https://web.archive.org/web/20190217214250/https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/) |
| Günlükler ve dökümler | - [Linux'ta Apache Hadoop İPLik uygulama günlüklerine erişin](hdinsight-hadoop-access-yarn-app-logs-linux.md)<br>- [Linux'ta Apache Hadoop hizmetleri için yığın dökümleri etkinleştirin](hdinsight-hadoop-collect-debug-heap-dump-linux.md)<br>- [HDInsight günlüklerini analiz edin](hdinsight-debug-jobs.md)|
| Hatalar | - [WebHCat hatalarını anlama ve çözme](hdinsight-hadoop-templeton-webhcat-debug-errors.md)<br>- [OutofMemory hatasını düzeltmek için Apache Hive ayarları](hdinsight-hadoop-hive-out-of-memory-error-oom.md) |
| Araçlar | - [Apache Hive sorgularını optimize edin](hdinsight-hadoop-optimize-hive-query.md)<br>- [HDInsight IntelliJ aracı](./spark/apache-spark-intellij-tool-plugin.md)<br>- [HDInsight Eclipse aracı](./spark/apache-spark-eclipse-tool-plugin.md)<br>- [HDInsight VSCode aracı](hdinsight-for-vscode.md)<br>- [HDInsight Visual Studio aracı](./hadoop/apache-hadoop-visual-studio-tools-get-started.md) |

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.