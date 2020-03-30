---
title: Azure HDInsight'ta Sorun Giderme Apache Spark
description: Apache Spark ve Azure HDInsight ile çalışma yla ilgili sık sorulan soruların yanıtlarını alın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: troubleshooting
ms.date: 08/22/2019
ms.custom: seodec18
ms.openlocfilehash: 80bca2dab1d07d9b99e75e283068bff99335fa18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271947"
---
# <a name="troubleshoot-apache-spark-by-using-azure-hdinsight"></a>Azure HDInsight kullanarak Apache Spark sorunlarını giderme

[Apache Ambari'de](https://ambari.apache.org/)Apache Spark yükleri ile çalışırken en önemli sorunlar ve bunların çözümleri hakkında bilgi edinin.

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters"></a>Kümeler üzerinde Apache Ambari kullanarak bir Apache Spark uygulamasını nasıl yapılandırabilirim?

Kıvılcım yapılandırma değerleri ayarlanabilir, Apache Spark `OutofMemoryError` uygulama özel durumlarını önlemeye yardımcı olur. Aşağıdaki adımlar Azure HDInsight'ta varsayılan Spark yapılandırma değerlerini gösterir:

1. Küme kimlik bilgilerinizle `https://CLUSTERNAME.azurehdidnsight.net` Ambari'ye giriş yapın. İlk ekranda genel bir pano görüntülenir. HDInsight 3.6 ve 4.0 arasında hafif kozmetik farklılıklar vardır.

1. **Spark2** > **Configs**gidin.

    ![Configs sekmesini seçin](./media/apache-troubleshoot-spark/apache-spark-ambari-config2.png)

1. Yapılandırmalar listesinde, **Özel kıvılcım2-varsayılanları**seçin ve genişletin.

1. **spark.executor.memory**gibi ayarlamanız gereken değer ayarını arayın. Bu durumda, **9728m** değeri çok yüksektir.

    ![Özel kıvılcım varsayılanlarını seçin](./media/apache-troubleshoot-spark/apache-spark-ambari-config4.png)

1. Değeri önerilen ayarına ayarlayın. Bu ayar için **2048m** değeri önerilir.

1. Değeri kaydedin ve yapılandırmayı kaydedin. **Kaydet'i**seçin.

    ![Değeri 2048m'e değiştirin](./media/apache-troubleshoot-spark/apache-spark-ambari-config6a.png)

    Yapılandırma değişiklikleri hakkında bir not yazın ve sonra **Kaydet'i**seçin.

    ![Yaptığınız değişiklikler le ilgili bir not girin](./media/apache-troubleshoot-spark/apache-spark-ambari-config6c.png)

    Herhangi bir yapılandırmanın dikkat çekmesi gerekiyorsa size bildirilir. Öğeleri not edin ve ardından **Yine de Devam Et'i**seçin.

    ![Yine de Devam Et'i seçin](./media/apache-troubleshoot-spark/apache-spark-ambari-config6b.png)

1. Bir yapılandırma kaydedildiğinde, hizmeti yeniden başlatmanız istenir. **Yeniden Başlat'ı**seçin.

    ![Yeniden Başlat'ı seçin](./media/apache-troubleshoot-spark/apache-spark-ambari-config7a.png)

    Yeniden başlatılını onaylayın.

    ![Tümünü Yeniden Başlat'ı Onayla'yı Seçin](./media/apache-troubleshoot-spark/apache-spark-ambari-config7b.png)

    Çalışan işlemleri gözden geçirebilirsiniz.

    ![Çalışan süreçlerini gözden geçirme](./media/apache-troubleshoot-spark/apache-spark-ambari-config7c.png)

1. Yapılandırmalar ekleyebilirsiniz. Yapılandırmalar listesinde, Özel **kıvılcım2 varsayılanlarını**seçin ve ardından **Özellik Ekle'yi**seçin.

    ![Özellik ekle'yi seçin](./media/apache-troubleshoot-spark/apache-spark-ambari-config8.png)

1. Yeni bir özellik tanımlayın. Veri türü gibi belirli ayarlar için bir iletişim kutusu kullanarak tek bir özellik tanımlayabilirsiniz. Veya satır başına bir tanım kullanarak birden çok özellik tanımlayabilirsiniz.

    Bu örnekte, **spark.driver.memory** özelliği **4g**değeri ile tanımlanır.

    ![Yeni özelliği tanımlama](./media/apache-troubleshoot-spark/apache-spark-ambari-config9.png)

1. Yapılandırmayı kaydedin ve ardından 6 ve 7 adımlarında açıklandığı gibi hizmeti yeniden başlatın.

Bu değişiklikler küme çapındadır, ancak Spark işini gönderdiğiniz zaman geçersiz kılınabilir.

## <a name="how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Kümeler üzerinde Jupyter notebook kullanarak bir Apache Spark uygulamasını nasıl yapılandırabilirim?

Jupyter not defterinin ilk hücresinde% **yapılandırma** yönergesi sonra, geçerli JSON biçiminde Spark yapılandırmaları belirtin. Gerçek değerleri gerektiği gibi değiştirin:

![Yapılandırma ekleme](./media/apache-troubleshoot-spark/add-configuration-cell.png)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters"></a>Kümeler üzerinde Apache Livy kullanarak bir Apache Spark uygulamasını nasıl yapılandırabilirim?

CURL gibi bir REST istemcisi kullanarak Spark uygulamasını Livy'ye gönderin. Aşağıdakilere benzer bir komut kullanın. Gerçek değerleri gerektiği gibi değiştirin:

```apache
curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
```

## <a name="how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters"></a>Kümeler üzerinde spark-submit kullanarak bir Apache Spark uygulamasını nasıl yapılandırabilirim?

Aşağıdakilere benzer bir komut kullanarak kıvılcım kabuğunu başlatın. Yapılandırmaların gerçek değerini gerektiği gibi değiştirin:

```apache
spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
```

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümelerinde Apache Spark iş gönderimi](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Kıvılcım bellek yönetimi genel bakış](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview).

* [HDInsight kümelerinde Hata Ayıklama Spark uygulaması.](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
