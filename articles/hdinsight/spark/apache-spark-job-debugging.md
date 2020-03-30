---
title: Azure HDInsight üzerinde çalışan Apache Spark işlerinin hatasını ayıklama
description: Azure HDInsight'ta Bir Kıvılcım kümesinde çalışan işleri izlemek ve hata ayıklamak için İPLik UI, Spark UI ve Spark History sunucusukullanın
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: bcf2f97e855126c86dbb1d74cd430704e2af3af1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75932128"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Azure HDInsight üzerinde çalışan Apache Spark işlerinin hatasını ayıklama

Bu makalede, [Apache Hadoop İPN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) UI, Spark UI ve Spark History Server'ı kullanarak HDInsight kümelerinde çalışan [Apache Spark](https://spark.apache.org/) işlerini izlemeyi ve hata ayıklamayı öğreneceksiniz. Spark kümesi, **Makine öğrenimi: MLLib kullanarak gıda denetim verileri üzerinde tahmine dayalı analiz**ile kullanılabilen bir dizüstü bilgisayar kullanarak bir Kıvılcım işine başlarsınız. Başka bir yaklaşım kullanarak gönderdiğiniz bir uygulamayı izlemek için aşağıdaki adımları kullanabilirsiniz, **örneğin, kıvılcım gönder.**

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

* Dizüstü bilgisayar, **[Makine öğrenme: MLLib kullanarak gıda denetim verileri üzerinde tahmine dayalı analiz](apache-spark-machine-learning-mllib-ipython.md)** çalıştırmaya başlamalıydı. Bu not defterinin nasıl çalıştırılaacağına ilişkin talimatlar için bağlantıyı izleyin.  

## <a name="track-an-application-in-the-yarn-ui"></a>İplik Kullanıcı UI'deki bir uygulamayı izleme

1. İplik UI'yi başlatın. **Küme panoları**altında **İplik'i** seçin.

    ![Azure portalı başlatma YARN UI](./media/apache-spark-job-debugging/launch-apache-yarn-ui.png)

   > [!TIP]  
   > Alternatif olarak, Ambari UI'den İplik UI'yi de başlatabilirsiniz. Ambari UI'yi başlatmak için **Küme panoları**altında **Ambari evini** seçin. Ambari UI itibaren, aktif Kaynak Yöneticisi > Kaynak **Yöneticisi UI**> **YARN** > **Hızlı Bağlantılar** gidin.

2. Jupyter dizüstü bilgisayarları kullanarak Kıvılcım işine başladığınız için, uygulama **remotesparkmagics** adını almıştır (bu not defterlerinden başlatılan tüm uygulamaların adıdır). İş hakkında daha fazla bilgi almak için uygulama adına göre uygulama kimliğini seçin. Bu, uygulama görünümünü başlatıyor.

    ![Spark geçmişi sunucusu Kıvılcım uygulama kimliğini bul](./media/apache-spark-job-debugging/find-application-id1.png)

    Jupyter dizüstü bilgisayarlardan başlatılan bu tür uygulamalar için, siz not defterinden çıkana kadar durum her zaman **çalışır.**

3. Uygulama görünümünden, uygulama ve günlükleri (stdout / stderr) ile ilişkili kapları bulmak için daha fazla ayrıntıya kadar ayrıntılı olabilir. Aşağıda gösterildiği gibi **İzleme URL'sine**karşılık gelen bağlantıyı tıklatarak Spark Kullanıcı Arabirimi'ni de başlatabilirsiniz.

    ![Spark geçmişi sunucu indirme konteyner günlükleri](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Spark Kullanıcı Yanı'ndaki bir uygulamayı izleme

Spark Kullanıcı Yanı'nda, daha önce başlattığınız uygulamatarafından ortaya çıkan Kıvılcım işlerine ayrıntılı bilgi verebilirsiniz.

1. Spark Kullanıcı UI'yi başlatmak için, uygulama görünümünden, yukarıdaki ekran görüntüsünde gösterildiği gibi **İzleme URL'sine**karşı bağlantıyı seçin. Jupyter dizüstü bilgisayarda çalışan uygulama tarafından başlatılan tüm Spark işleri görebilirsiniz.

    ![Spark geçmişi sunucu işleri sekmesi](./media/apache-spark-job-debugging/view-apache-spark-jobs.png)

2. Her uygulayıcıiçin işleme ve depolama bilgilerini görmek için **Uygulayıcılar** sekmesini seçin. **İş Parçacığı Dökümü** bağlantısını seçerek arama yığınını da alabilirsiniz.

    ![Spark geçmişi sunucu uygulayıcıları sekmesi](./media/apache-spark-job-debugging/view-spark-executors.png)

3. Uygulamayla ilişkili aşamaları görmek için **Aşamalar** sekmesini seçin.

    ![Spark geçmişi sunucu aşamaları sekmesi](./media/apache-spark-job-debugging/view-apache-spark-stages.png "Kıvılcım aşamalarını görüntüleyin")

    Her aşamada, aşağıda gösterildiği gibi yürütme istatistiklerini görüntüleyebileceğiniz birden çok görev olabilir.

    ![Spark geçmişi sunucu aşamaları sekmesi ayrıntıları](./media/apache-spark-job-debugging/view-spark-stages-details.png "Kıvılcım aşamaları ayrıntılarını görüntüleyin")

4. Sahne ayrıntıları sayfasından DAG Visualization'ı başlatabilirsiniz. Aşağıda gösterildiği gibi, sayfanın üst kısmındaki **DAG Görselleştirme** bağlantısını genişletin.

    ![Kıvılcım aşamalarını görüntüle DAG görselleştirme](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)

    DAG veya Doğrudan Aclyic Graph uygulamanın farklı aşamalarını temsil eder. Grafikteki her mavi kutu, uygulamadan çağrılan bir Kıvılcım işlemini temsil eder.

5. Aşama ayrıntıları sayfasından, uygulama zaman çizelgesi görünümünü de başlatabilirsiniz. Aşağıda gösterildiği gibi sayfanın üst kısmındaki **Olay Zaman Çizelgesi** bağlantısını genişletin.

    ![Spark aşamaları olay zaman çizelgesini görüntüleme](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)

    Bu, Kıvılcım olaylarını bir zaman çizelgesi biçiminde görüntüler. Zaman çizelgesi görünümü, işler arasında, bir iş içinde ve bir aşama içinde üç düzeyde kullanılabilir. Yukarıdaki resim, belirli bir aşamanın zaman çizelgesi görünümünü yakalar.

   > [!TIP]  
   > **Yakınlaştırmayı Etkinleştir** onay kutusunu seçerseniz, zaman çizelgesi görünümünde sola ve sağa kaydırabilirsiniz.

6. Spark Kullanıcı Arabirimi'ndeki diğer sekmeler de Spark örneği hakkında yararlı bilgiler sağlar.

   * Depolama sekmesi - Uygulamanız bir RDD oluşturuyorsa, Depolama sekmesinde bunlar hakkında bilgi bulabilirsiniz.
   * Çevre sekmesi - Bu sekme, Aşağıdakiler gibi Kıvılcım örneğiniz hakkında yararlı bilgiler sağlar:
     * Scala sürümü
     * Kümeyle ilişkili olay günlüğü dizini
     * Uygulama için uygulayıcı çekirdek sayısı
     * Etc.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Spark History Server'ı kullanarak tamamlanan işler hakkında bilgi edinin

Bir iş tamamlandıktan sonra, iş hakkındaki bilgiler Spark History Server'da devam eder.

1. **Genel Bakış** sayfasından Spark History Server'ı başlatmak için **Küme panoları**altında **Spark geçmişi sunucusunu** seçin.

    ![Azure portalı Spark geçmişi sunucusubaşlatın](./media/apache-spark-job-debugging/launch-spark-history-server.png "Fırlatma Kıvılcım Geçmişi Server1")

   > [!TIP]  
   > Alternatif olarak, Ambari UI'den Spark History Server UI'yi de başlatabilirsiniz. Ambari UI'yi başlatmak için, Genel Bakış bıçağından, **Küme panoları**altında **Ambari evini** seçin. Ambari UI itibaren, **Spark2** > **Hızlı Bağlantılar** > **Spark2 History Server UI**gidin.

2. Listelenen tüm tamamlanmış uygulamaları görürsünüz. Daha fazla bilgi için bir uygulamaya sondaj yapmak için bir uygulama kimliği seçin.

    ![Spark geçmişi sunucusu uygulamaları tamamladı](./media/apache-spark-job-debugging/view-completed-applications.png "Fırlatma Kıvılcım Tarihi Server2")

## <a name="see-also"></a>Ayrıca bkz.

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Genişletilmiş Spark Geçmişi Sunucusu'nun Yanık Apache Spark İşleri](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>Veri analistleri için

* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight'ta Apache Spark kullanarak Uygulama Öngörüsü telemetri veri analizi](apache-spark-analyze-application-insight-logs.md)


### <a name="for-spark-developers"></a>Kıvılcım geliştiricileri için

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ IDEA için HDInsight Araçları Eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)
