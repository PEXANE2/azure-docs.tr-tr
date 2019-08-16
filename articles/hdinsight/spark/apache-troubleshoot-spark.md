---
title: Azure HDInsight Spark sorunlarını giderme
description: Apache Spark ve Azure HDInsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.custom: seodec18
ms.openlocfilehash: c88136fee7a75b8f3b8e504b1ff1e6673a31bcf7
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543167"
---
# <a name="troubleshoot-apache-spark-by-using-azure-hdinsight"></a>Apache Spark, Azure HDInsight'ı kullanarak sorun giderme

İle çalışırken sık karşılaşılan sorunlar ve çözümleri hakkında bilgi edinin [Apache Spark](https://spark.apache.org/) yüklerde [Apache Ambari](https://ambari.apache.org/).

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters"></a>Bir Apache Spark uygulaması kümeleri üzerinde Apache Ambari kullanarak nasıl yapılandırabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

Spark yapılandırma değerleri ayarlanabilir Apache Spark bir Application OutofMemoryError özel durumu kullanmaktan kaçınmaya yardımcı olur. Aşağıdaki adımlar, Azure HDInsight 'ta varsayılan Spark yapılandırma değerlerini göstermektedir: 

1. Kümeleri listesinde seçin **Spark2**.

    ![Küme listeden seçin.](./media/apache-troubleshoot-spark/update-config-1.png)

2. Seçin **yapılandırmaları** sekmesi.

    ![Yapılandırmaları sekmesini seçin](./media/apache-troubleshoot-spark/update-config-2.png)

3. Listeden yapılandırmaları seçin **özel spark2 varsayılanları**.

    ![Özel spark Varsayılanları seçin](./media/apache-troubleshoot-spark/update-config-3.png)

4. Gibi ayarlamak için gereken değer ayarı Ara **spark.executor.memory**. Bu durumda, değerini **4608m** çok yüksek.

    ![Spark.executor.memory alanı seçin](./media/apache-troubleshoot-spark/update-config-4.png)

5. Önerilen ayar için değer ayarlayın. Değer **2048m** için bu ayar önerilir.

    ![Değeri değiştirmek için 2048 m](./media/apache-troubleshoot-spark/update-config-5.png)

6. Değer kaydedin ve ardından yapılandırmayı kaydedin. Araç çubuğunda **Kaydet**.

    ![Ayar ve yapılandırmayı kaydedin](./media/apache-troubleshoot-spark/update-config-6a.png)

    Tüm yapılandırmaları dikkat etmeniz gerekiyorsa size bildirilir. Öğeleri not edin ve ardından **yine de devam**. 

    ![Select yine de devam](./media/apache-troubleshoot-spark/update-config-6b.png)

    Yapılandırma değişiklikleri hakkında bir not yazın ve ardından **Kaydet**.

    ![Yaptığınız değişiklikleri hakkında bir not girin](./media/apache-troubleshoot-spark/update-config-6c.png)

7. Bir yapılandırma kaydedildiği zaman hizmeti yeniden başlatmanız istenir. Seçin **yeniden**.

    ![Yeniden başlatma seçin](./media/apache-troubleshoot-spark/update-config-7a.png)

    Yeniden başlatma işlemini onaylayın.

    ![Tüm yeniden Onayla seçeneğini belirleyin](./media/apache-troubleshoot-spark/update-config-7b.png)

    Çalışan işlemler gözden geçirebilirsiniz.

    ![Çalışan işlemleri gözden geçirin](./media/apache-troubleshoot-spark/update-config-7c.png)

8. Yapılandırmaları ekleyebilirsiniz. Listeden yapılandırmaları seçin **özel spark2 varsayılanları**ve ardından **Özellik Ekle**.

    ![Özellik Ekle'yi seçin](./media/apache-troubleshoot-spark/update-config-8.png)

9. Yeni bir özellik tanımlayın. Veri türü gibi belirli ayarlar için bir iletişim kutusunu kullanarak, tek bir özellik tanımlayabilirsiniz. Veya, her satırda bir tanım'ı kullanarak birden çok özellik tanımlayabilirsiniz. 

    Bu örnekte, **spark.driver.memory** özellik değeriyle tanımlanan **4g**.

    ![Yeni bir özellik tanımlayın](./media/apache-troubleshoot-spark/update-config-9.png)

10. Yapılandırmayı kaydetmek ve 6 ve 7. adımda açıklandığı hizmeti yeniden başlatın.

Bu değişiklikler, küme çapında ancak Spark işi gönderdiğinizde geçersiz kılınabilir.

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümeleri üzerinde Apache Spark iş gönderme](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Bir Apache Spark uygulaması kümeleri Jupyter Not Defteri kullanarak nasıl yapılandırabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark yapılandırmalarının ayarlanması gerektiğini ve değerleri belirlemek için, bkz. Apache Spark Application OutofMemoryError özel durumu.

2. Jupyter Not Defteri, ilk hücrenin sonra **%% yapılandırma** yönergesi, Spark yapılandırmaları geçerli JSON biçiminde belirtin. Gerçek değerleri gerektiği gibi değiştirin:

    ![Yapılandırma Ekle](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümeleri üzerinde Apache Spark iş gönderme](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters"></a>Bir Apache Spark uygulaması kümeleri üzerinde Apache Livy kullanarak nasıl yapılandırabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark yapılandırmalarının ayarlanması gerektiğini ve değerleri belirlemek için, bkz. Apache Spark Application OutofMemoryError özel durumu. 

2. Spark uygulaması Livy için cURL gibi bir REST istemcisi kullanarak gönderin. Aşağıdakine benzer bir komut kullanın. Gerçek değerleri gerektiği gibi değiştirin:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümeleri üzerinde Apache Spark iş gönderme](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters"></a>Kullanarak uygulama spark-submit bir Apache Spark kümelerinde nasıl yapılandırabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark yapılandırmalarının ayarlanması gerektiğini ve değerleri belirlemek için, bkz. Apache Spark Application OutofMemoryError özel durumu.

2. Spark-shell, aşağıdakine benzer bir komut kullanarak başlatın. Gerçek değer yapılandırmalarının gerektiği gibi değiştirin: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümeleri üzerinde Apache Spark iş gönderme](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Spark bellek yönetimine genel bakış](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview).

* [HDInsight kümelerinde Spark uygulamasında hata ayıklama](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/).

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
