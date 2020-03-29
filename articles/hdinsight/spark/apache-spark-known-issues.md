---
title: Azure HDInsight'ta Apache Spark kümesiyle ilgili sorun giderme sorunları
description: Azure HDInsight'taki Apache Spark kümeleri ile ilgili sorunlar ve bunların nasıl çözüme bu şekilde çözüm eleştirilen konular hakkında bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.author: hrasheed
ms.openlocfilehash: 2c153d818136c5d8804dae72004dfaf17fd1bf7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73494521"
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>HDInsight'ta Apache Spark kümesi için bilinen sorunlar

Bu belge, HDInsight Spark genel önizlemesi için bilinen tüm sorunları izler.  

## <a name="apache-livy-leaks-interactive-session"></a>Apache Livy interaktif oturum sızdırıyor
[Apache Livy](https://livy.incubator.apache.org/) yeniden başlatTığında [(Apache Ambari'den](https://ambari.apache.org/) veya headnode 0 sanal makinenin yeniden başlatılması ndan dolayı) interaktif bir oturum hala canlıyken, etkileşimli bir iş oturumu sızdırılır. Sonuç olarak, yeni işler Kabul edilen durumda sıkışmış olabilir.

**Azaltma:**

Sorunu çözmek için aşağıdaki yordamı kullanın:

1. Headnode içine Ssh. Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Livy aracılığıyla başlatılan etkileşimli işlerin uygulama işlerini bulmak için şeni yürüt

        yarn application –list

    İşler açık adlar belirtilmeyen bir Livy etkileşimli oturumla başlatılırsa varsayılan iş adları Livy olacaktır. [Jupyter Notebook](https://jupyter.org/)tarafından başlatılan Livy oturumu için, `remotesparkmagics_*`iş adı ile başlar.

3. Bu işleri öldürmek için aşağıdaki komutu çalıştırın.

        yarn application –kill <Application ID>

Yeni işler çalışmaya başlar.

## <a name="spark-history-server-not-started"></a>Spark History Server başlatılmamadı
Bir küme oluşturulduktan sonra Spark History Server otomatik olarak başlatılmaz.  

**Azaltma:**

Ambari'den tarih sunucusunu el ile başlatın.

## <a name="permission-issue-in-spark-log-directory"></a>Spark günlük dizininde izin sorunu
hdiuser kıvılcım gönder kullanarak bir iş gönderirken aşağıdaki hatayı alır:

```
java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied)
```

Ve sürücü günlüğü yazmıyor.

**Azaltma:**

1. Hadoop grubuna hdiuser ekleyin.
2. Küme oluşturmadan sonra /var/log/spark'ta 777 izin sağlayın.
3. Ambari'yi kullanarak 777 izinli bir dizin olarak kıvılcım günlüğü konumunu güncelleştirin.  
4. Kıvılcım-sudo olarak gönderin.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Kıvılcım-Phoenix konektörü desteklenmiyor

HDInsight Spark kümeleri Spark-Phoenix konektörünü desteklemez.

**Azaltma:**

Bunun yerine Spark-HBase konektörünü kullanmanız gerekir. Talimatlar için Bkz. [Spark-HBase konektörü nasıl kullanılır.](https://web.archive.org/web/20190112153146/https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/)

## <a name="issues-related-to-jupyter-notebooks"></a>Jupyter dizüstü bilgisayarlarla ilgili sorunlar

Jupyter dizüstü bilgisayarlarla ilgili bilinen bazı sorunlar aşağıda veda edilebistir.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Dosya adlarında ASCII olmayan karakterlere sahip not defterleri

Jupyter not defteri dosya adlarında ASCII olmayan karakterler kullanmayın. ASCII olmayan bir dosya adı olan Jupyter UI aracılığıyla bir dosya yüklemeye çalışırsanız, herhangi bir hata iletisi olmadan başarısız olur. Jupyter dosyayı yüklemenize izin vermez, ancak görünür bir hata da atmaz.

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Daha büyük boyutlardaki defterleri yüklerken hata

Boyutu daha büyük **`Error loading notebook`** olan not defterlerini yüklerken bir hata görebilirsiniz.  

**Azaltma:**

Bu hatayı alırsanız, verilerinizin bozuk veya kayıp olduğu anlamına gelmez.  Not defterleriniz hala diskte `/var/lib/jupyter`ve bunlara erişmek için kümeye SSH'yi girebilirsiniz. Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

SSH kullanarak kümeye bağlandıktan sonra, not defterinizdeki önemli verilerin kaybolmasını önlemek için not defterlerinizi kümenizden yerel makinenize (SCP veya WinSCP kullanarak) kopyalayabilirsiniz. Daha sonra ssh tünel port 8001 de geçit girmeden Jupyter erişmek için headnode içine olabilir.  Buradan, dizüstü bilgisayarınızın çıktısını temizleyebilir ve dizüstü bilgisayarın boyutunu en aza indirmek için yeniden kaydedebilirsiniz.

Bu hatanın gelecekte oluşmasını önlemek için bazı en iyi uygulamaları izlemeniz gerekir:

* Not defteri boyutunu küçük tutmak önemlidir. Jupyter'a geri gönderilen Kıvılcım işlerinizden elde edilen tüm çıktılar not defterinde kalıcıdır.  Büyük RDD veya veri çerçeveleri üzerinde `.collect()` çalışan önlemek için genel olarak Jupyter ile en iyi uygulamadır; bunun yerine, bir RDD'nin içeriğine göz `.take()` atmak `.sample()` istiyorsanız, çıktınızın çok büyük olmaması için çalıştırmayı veya çalıştırmayı düşünün.
* Ayrıca, bir dizüstü bilgisayar kaydettiğinizde, boyutu azaltmak için tüm çıktı hücrelerini temizleyin.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Not defterinin ilk başlatması beklenenden daha uzun sürüyor

Jupyter not defterinde Spark büyüsünü kullanan ilk kod deyimi bir dakikadan fazla sürebilir.  

**Açıklama:**

Bunun nedeni, ilk kod hücresi çalıştırıldığında gerçekleşsin. Arka planda bu oturum yapılandırmasını başlatır ve Spark, SQL ve Hive bağlamları ayarlanır. Bu bağlamlar ayarlandıktan sonra, ilk deyim çalıştırılır ve bu, deyimin tamamlanmasının uzun zaman aldığı izlenimini verir.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Oturumu oluştururken jupyter dizüstü bilgisayar zaman

Kıvılcım kümesi nin kaynakları bittiğinde, Jupyter not defterindeki Kıvılcım ve PySpark çekirdekleri oturumu oluşturmaya çalışırken zaman zaman alacaktır.

**Azaltıcı etken:**

1. Spark kümenizdeki bazı kaynakları şu şekilde serbest

   * Kapat ve Durdur menüsüne giderek veya not defteri gezgininde Kapat'ı tıklatarak diğer Spark dizüstü bilgisayarları durdurma.
   * İplik'ten diğer Kıvılcım uygulamalarını durdurma.

2. Başlatmaya çalıştığınız not defterini yeniden başlatın. Şimdi bir oturum oluşturmak için yeterli kaynak kullanılabilir olmalıdır.

## <a name="see-also"></a>Ayrıca bkz.

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ IDEA için HDInsight Araçları Eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)