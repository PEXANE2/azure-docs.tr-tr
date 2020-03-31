---
title: Azure HDInsight'ta Apache Spark kümesi için kaynakları yönetme
description: Daha iyi performans için Azure HDInsight'ta Spark kümeleri için kaynakları nasıl yöneteceklerini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 3aab89f86dcd48328771cd0fda03d1c9de4bc2c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75932103"
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Azure HDInsight'ta Apache Spark kümesi için kaynakları yönetme

[Apache Ambari](https://ambari.apache.org/) UI, [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) UI ve [Apache Spark](https://spark.apache.org/) kümenizle ilişkili [Spark History Server](./apache-azure-spark-history-server.md) gibi arabirimlere nasıl erişeceğinizi ve en iyi performans için küme yapılandırmasını nasıl ayarladığınıöğrenin.

## <a name="open-the-spark-history-server"></a>Kıvılcım Geçmişi Sunucusunu Aç

Spark History Server, Tamamlanan ve çalıştırılan Spark uygulamaları için web Kullanıcı Arabirimi'dir. Spark'ın Web arama hizmetinin bir uzantısı. Tam bilgi için [Spark History Server'a](./apache-azure-spark-history-server.md)bakın.

## <a name="open-the-yarn-ui"></a>İplik UI'yi aç

Şu anda Kıvılcım kümesiüzerinde çalışan uygulamaları izlemek için İplik Kullanıcı Birası'nı kullanabilirsiniz.

1. Azure [portalından](https://portal.azure.com/)Spark kümesini açın. Daha fazla bilgi için liste [ve kümeleri göster'e](../hdinsight-administer-use-portal-linux.md#showClusters)bakın.

2. **Küme panolarından** **İplik'i**seçin. İstendiğinde, Spark kümesi için yönetici kimlik bilgilerini girin.

    ![Fırlatma İplik UI](./media/apache-spark-resource-manager/azure-portal-dashboard-yarn.png)

   > [!TIP]  
   > Alternatif olarak, Ambari UI'den İplik UI'yi de başlatabilirsiniz. Ambari UI itibaren, **YARN** > **Hızlı Bağlantılar** > **Active** > **Resource Manager UI**gidin.

## <a name="optimize-clusters-for-spark-applications"></a>Spark uygulamaları için kümeleri optimize edin

Uygulama gereksinimlerine bağlı olarak Kıvılcım yapılandırması için kullanılabilecek `spark.executor.instances`üç `spark.executor.cores`temel `spark.executor.memory`parametre , ve . Uygulayıcı, Bir Kıvılcım uygulaması için başlatılan bir işlemdir. İşçi düğümüüzerinde çalışır ve uygulama nın görevlerini yerine getirmekle yükümlüdür. Her küme için varsayılan uygulayıcı sayısı ve uygulayıcı boyutları, alt düğüm sayısı ve alt düğüm boyutuna göre hesaplanır. Bu bilgiler küme `spark-defaults.conf` kafa düğümlerinde depolanır.

Üç yapılandırma paramı küme düzeyinde (küme üzerinde çalışan tüm uygulamalar için) yapılandırılabilir veya her bir uygulama için de belirtilebilir.

### <a name="change-the-parameters-using-ambari-ui"></a>Ambari UI kullanarak parametreleri değiştirme

1. Ambari UI'den **Spark2** > **Configs** > **Custom spark2-varsayılanlara**gidin.

    ![Ambari özel kullanarak parametreleri ayarlama](./media/apache-spark-resource-manager/ambari-ui-spark2-configs.png "Ambari özel kullanarak parametreleri ayarlama")

1. Varsayılan değerler kümeüzerinde aynı anda çalışan dört Spark uygulamasına sahip olmak için iyidir. Aşağıdaki ekran görüntüsünde gösterildiği gibi, bu değerleri kullanıcı arabiriminden değiştirebilirsiniz:

    ![Ambari kullanarak parametreleri ayarlama](./media/apache-spark-resource-manager/ambari-ui-spark2-defaults.png "Ambari kullanarak parametreleri ayarlama")

1. Yapılandırma değişikliklerini kaydetmek için **Kaydet'i** seçin. Sayfanın üst kısmında, etkilenen tüm hizmetleri yeniden başlatmanız istenir. **Yeniden Başlat'ı**seçin.

    ![Hizmetleri yeniden başlatın](./media/apache-spark-resource-manager/apache-ambari-restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter dizüstü bilgisayarda çalışan bir uygulamanın parametrelerini değiştirme

Jupyter dizüstü bilgisayarda çalışan uygulamalar için, `%%configure` yapılandırma değişiklikleri yapmak için sihirli kullanabilirsiniz. İdeal olarak, ilk kod hücrenizi çalıştırmadan önce uygulamanın başında bu tür değişiklikler yapmalısınız. Bunu yapmak, yapılandırmanın oluşturulduğunda Livy oturumuna uygulanmasını sağlar. Yapılandırmayı uygulamanın sonraki bir aşamasında değiştirmek istiyorsanız, parametreyi `-f` kullanmanız gerekir. Ancak, böylece uygulamada tüm ilerleme kaybolur.

Aşağıdaki parçacık, Jupyter'da çalışan bir uygulamanın yapılandırmasını nasıl değiştireceğini gösterir.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Yapılandırma parametreleri JSON dizesi olarak geçirilmeli ve örnek sütunda gösterildiği gibi büyüden sonra bir sonraki satırda olmalıdır.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Kıvılcım gönder kullanılarak gönderilen bir uygulamanın parametrelerini değiştirme

Aşağıdaki komut, kullanılarak `spark-submit`gönderilen toplu iş uygulaması için yapılandırma parametrelerinin nasıl değiştirileceklerine ilişkin bir örnektir.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>cURL kullanılarak gönderilen bir uygulamanın parametrelerini değiştirme

Aşağıdaki komut, cURL kullanılarak gönderilen bir toplu iş uygulaması için yapılandırma parametrelerinin nasıl değiştirileceklerine ilişkin bir örnektir.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="change-these-parameters-on-a-spark-thrift-server"></a>Bu parametreleri Bir Kıvılcım Tutumlu Sunucusunda değiştirme

Spark Thrift Server, Bir Spark kümesine JDBC/ODBC erişimi sağlar ve Spark SQL sorgularını hizmet etmek için kullanılır. Power BI, Tableau ve benzeri araçlar, Spark Thrift Server ile iletişim kurmak için ODBC protokolünü kullanarak Sql sorgularını Bir Kıvılcım Uygulaması olarak çalıştırın. Bir Kıvılcım kümesi oluşturulduğunda, Kıvılcım Tutumlu Sunucusu'nun iki örneği başlatılır, her baş düğümünde bir tane. Her Kıvılcım Tutumlu Sunucu, İplik Kullanıcı Arabirimi'nde Bir Kıvılcım uygulaması olarak görülebilir.

Spark Thrift Server, Spark dinamik yürütme `spark.executor.instances` alanı kullanır ve bu nedenle kullanılmaz. Bunun yerine, Spark `spark.dynamicAllocation.maxExecutors` Thrift Server kullanır ve `spark.dynamicAllocation.minExecutors` uygulayıcı sayısını belirtmek için. Yapılandırma parametreleri `spark.executor.cores` `spark.executor.memory` ve yürütücü boyutunu değiştirmek için kullanılır. Aşağıdaki adımlarda gösterildiği gibi bu parametreleri değiştirebilirsiniz:

* Parametreleri `spark.dynamicAllocation.maxExecutors`güncellemek için **Gelişmiş spark2-tutumlu-sparkconf** kategorisini genişletin ve `spark.dynamicAllocation.minExecutors`.

    ![Kıvılcım tutumlu sunucuyapılandırma](./media/apache-spark-resource-manager/ambari-ui-advanced-thrift-sparkconf.png "Kıvılcım tutumlu sunucuyapılandırma")

* Parametreleri `spark.executor.cores`güncellemek için **Özel spark2-tutumlu-sparkconf** kategorisini genişletin ve `spark.executor.memory`.

    ![Spark tutumlu sunucu parametresi yapılandırma](./media/apache-spark-resource-manager/ambari-ui-custom-thrift-sparkconf.png "Spark tutumlu sunucu parametresi yapılandırma")

### <a name="change-the-driver-memory-of-the-spark-thrift-server"></a>Kıvılcım Tutumlu Sunucusunun sürücü belleği değiştirme

Kıvılcım Thrift Server sürücü belleği, kafa düğümünün toplam RAM boyutu 14 GB'dan büyük olması koşuluyla, kafa düğümü RAM boyutunun %25'ine göre yapılandırılır. Aşağıdaki ekran görüntüsünde gösterildiği gibi, sürücü bellek yapılandırmasını değiştirmek için Ambari UI'yi kullanabilirsiniz:

Ambari UI itibaren, **Spark2** > **Configs** > **Gelişmiş spark2-env**gidin. Sonra **spark_thrift_cmd_opts**için değer sağlamak.

## <a name="reclaim-spark-cluster-resources"></a>Spark küme kaynaklarını geri alma

Spark dinamik ayırma nedeniyle, tutumlu sunucu tarafından tüketilen tek kaynak iki uygulama yöneticisi için kaynaklardır. Bu kaynakları geri almak için kümede çalışan Thrift Server hizmetlerini durdurmanız gerekir.

1. Ambari UI'den, sol bölmeden **Spark2'yi**seçin.

2. Sonraki sayfada, **Spark2 Thrift Sunucuları'nı**seçin.

    ![Tutumlusunucu'yı yeniden başlat1](./media/apache-spark-resource-manager/ambari-ui-spark2-thrift-servers.png "Tutumlusunucu'yı yeniden başlat1")

3. Spark2 Thrift Server'ın üzerinde çalışan iki başlığı görmelisiniz. Headnodes birini seçin.

    ![Tutumlusunucu2'yi yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-2.png "Tutumlusunucu2'yi yeniden başlatın")

4. Bir sonraki sayfada o düğümde çalışan tüm hizmetler listelanıyor. Listeden, Spark2 Thrift Server'ın yanındaki açılır düğmeyi seçin ve ardından **Durdur'u**seçin.

    ![Tutumlusunucu3'u yeniden başlatın](./media/apache-spark-resource-manager/ambari-ui-spark2-thriftserver-restart.png "Tutumlusunucu3'u yeniden başlatın")
5. Diğer başlık düğümde de bu adımları tekrarlayın.

## <a name="restart-the-jupyter-service"></a>Jupyter hizmetini yeniden başlatın

Makalenin başında gösterildiği gibi Ambari Web UI başlatın. Sol daki gezinti bölmesinden **Jupyter'ı**seçin, **Hizmet Eylemleri'ni**seçin ve ardından **Tümünü Yeniden Başlat'ı**seçin. Bu jupyter servisini tüm kafa düğümlerinde başlatır.

![Jupyter’i yeniden başlatın](./media/apache-spark-resource-manager/apache-ambari-restart-jupyter.png "Jupyter’i yeniden başlatın")

## <a name="monitor-resources"></a>Kaynakları izleme

Makalenin başında gösterildiği gibi İplik UI başlatın. Ekranın üst kısmındaki Küme Ölçümleri tablosunda, **Kullanılan Bellek** ve **Bellek Toplam** sütunlarının değerlerini denetleyin. İki değer yakınsa, bir sonraki uygulamayı başlatmak için yeterli kaynak olmayabilir. Aynı kullanılan **VCores** ve **VCores Toplam sütunlar** için de geçerlidir. Ayrıca, ana görünümde, kabul **edilen** durumda kalan ve **RUNNING** veya **FAILED** durumuna geçmeyen bir uygulama varsa, bu da başlamak için yeterli kaynak elde etmediğinin bir göstergesi olabilir.

![Kaynak Sınırı](./media/apache-spark-resource-manager/apache-ambari-resource-limit.png "Kaynak Sınırı")

## <a name="kill-running-applications"></a>Çalışan uygulamaları öldürme

1. İplik UI'sinde, sol panelden **Çalışan'ı**seçin. Çalışan uygulamalar listesinden, öldürülecek uygulamayı belirleyin ve **kimliği**seçin.

    ![Uygulamayı Öldür1](./media/apache-spark-resource-manager/apache-ambari-kill-app1.png "Uygulamayı Öldür1")

2. Sağ üst köşede **Uygulamayı Öldür'i** seçin ve **ardından Tamam'ı**seçin.

    ![App2 öldür](./media/apache-spark-resource-manager/apache-ambari-kill-app2.png "App2 öldür")

## <a name="see-also"></a>Ayrıca bkz.

* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Veri analistleri için

* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight'ta Apache Spark kullanarak Uygulama Öngörüsü telemetri veri analizi](apache-spark-analyze-application-insight-logs.md)

### <a name="for-apache-spark-developers"></a>Apache Spark geliştiricileri için

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ IDEA için HDInsight Araçları Eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)
