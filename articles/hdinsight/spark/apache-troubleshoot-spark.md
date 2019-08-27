---
title: Azure HDInsight 'ta Apache Spark sorunlarını giderme
description: Apache Spark ve Azure HDInsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: troubleshooting
ms.date: 08/22/2019
ms.custom: seodec18
ms.openlocfilehash: 143a941b8266734a8415c71daafc07681bc13b80
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70049592"
---
# <a name="troubleshoot-apache-spark-by-using-azure-hdinsight"></a>Apache Spark, Azure HDInsight'ı kullanarak sorun giderme

[Apache ambarı](https://ambari.apache.org/)'nda Apache Spark yükleri ile çalışırken en üstteki sorunlar ve çözümleri hakkında bilgi edinin.

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters"></a>Bir Apache Spark uygulaması kümeleri üzerinde Apache Ambari kullanarak nasıl yapılandırabilirim?

Spark yapılandırma değerleri, Apache Spark bir uygulama `OutofMemoryError` özel durumuyla karşılaşmamak için ayarlanabilir. Aşağıdaki adımlar, Azure HDInsight 'ta varsayılan Spark yapılandırma değerlerini göstermektedir:

1. Küme kimlik bilgilerinizle `https://CLUSTERNAME.azurehdidnsight.net` , ambarı ' nda oturum açın. İlk ekranda Genel Bakış Panosu görüntülenir. HDInsight 3,6 ve 4,0 arasında hafif yüzeysel farklılıkları vardır.

1. **Spark2** > **configs**sayfasına gidin.

    ![Yapılandırmaları sekmesini seçin](./media/apache-troubleshoot-spark/update-config-2.png)

1. Yapılandırma listesinde **Custom-spark2-Defaults**' ı seçin ve genişletin.

1. Gibi ayarlamak için gereken değer ayarı Ara **spark.executor.memory**. Bu durumda, **9728dk** değeri çok yüksektir.

    ![Özel spark Varsayılanları seçin](./media/apache-troubleshoot-spark/update-config-4.png)

1. Önerilen ayar için değer ayarlayın. Değer **2048m** için bu ayar önerilir.

1. Değer kaydedin ve ardından yapılandırmayı kaydedin. **Kaydet**’i seçin.

    ![Değeri değiştirmek için 2048 m](./media/apache-troubleshoot-spark/update-config-6a.png)

    Yapılandırma değişiklikleri hakkında bir not yazın ve ardından **Kaydet**.

    ![Yaptığınız değişiklikleri hakkında bir not girin](./media/apache-troubleshoot-spark/update-config-6c.png)

    Tüm yapılandırmaları dikkat etmeniz gerekiyorsa size bildirilir. Öğeleri not edin ve ardından **yine de devam**.

    ![Select yine de devam](./media/apache-troubleshoot-spark/update-config-6b.png)

1. Bir yapılandırma kaydedildiği zaman hizmeti yeniden başlatmanız istenir. Seçin **yeniden**.

    ![Yeniden başlatma seçin](./media/apache-troubleshoot-spark/update-config-7a.png)

    Yeniden başlatma işlemini onaylayın.

    ![Tüm yeniden Onayla seçeneğini belirleyin](./media/apache-troubleshoot-spark/update-config-7b.png)

    Çalışan işlemler gözden geçirebilirsiniz.

    ![Çalışan işlemleri gözden geçirin](./media/apache-troubleshoot-spark/update-config-7c.png)

1. Yapılandırmaları ekleyebilirsiniz. Listeden yapılandırmaları seçin **özel spark2 varsayılanları**ve ardından **Özellik Ekle**.

    ![Özellik Ekle'yi seçin](./media/apache-troubleshoot-spark/update-config-8.png)

1. Yeni bir özellik tanımlayın. Veri türü gibi belirli ayarlar için bir iletişim kutusunu kullanarak, tek bir özellik tanımlayabilirsiniz. Veya, her satırda bir tanım'ı kullanarak birden çok özellik tanımlayabilirsiniz.

    Bu örnekte, **spark.driver.memory** özellik değeriyle tanımlanan **4g**.

    ![Yeni bir özellik tanımlayın](./media/apache-troubleshoot-spark/update-config-9.png)

1. Yapılandırmayı kaydetmek ve 6 ve 7. adımda açıklandığı hizmeti yeniden başlatın.

Bu değişiklikler, küme çapında ancak Spark işi gönderdiğinizde geçersiz kılınabilir.

## <a name="how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Bir Apache Spark uygulaması kümeleri Jupyter Not Defteri kullanarak nasıl yapılandırabilirim?

Jupyter Not Defteri, ilk hücrenin sonra **%% yapılandırma** yönergesi, Spark yapılandırmaları geçerli JSON biçiminde belirtin. Gerçek değerleri gerektiği gibi değiştirin:

![Yapılandırma Ekle](./media/apache-troubleshoot-spark/add-configuration-cell.png)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters"></a>Bir Apache Spark uygulaması kümeleri üzerinde Apache Livy kullanarak nasıl yapılandırabilirim?

Spark uygulaması Livy için cURL gibi bir REST istemcisi kullanarak gönderin. Aşağıdakine benzer bir komut kullanın. Gerçek değerleri gerektiği gibi değiştirin:

```apache
curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
```

## <a name="how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters"></a>Kullanarak uygulama spark-submit bir Apache Spark kümelerinde nasıl yapılandırabilirim?

Spark-shell, aşağıdakine benzer bir komut kullanarak başlatın. Gerçek değer yapılandırmalarının gerektiği gibi değiştirin:

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
