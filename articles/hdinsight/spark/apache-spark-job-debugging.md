---
title: Azure HDInsight üzerinde çalışan Apache Spark işlerinin hatasını ayıklama
description: Azure HDInsight 'ta bir Spark kümesinde çalışan işleri izlemek ve hatalarını ayıklamak için YARN UI, Spark UI ve Spark geçmiş sunucusunu kullanın
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/23/2020
ms.openlocfilehash: 76c52f0fd31b61937f04946db998088d978e1bc8
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82117429"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Azure HDInsight üzerinde çalışan Apache Spark işlerinin hatasını ayıklama

Bu makalede, HDInsight kümelerinde çalışan Apache Spark işlerin nasıl izleneceğini ve hata ayıklanalınacağını öğreneceksiniz. Apache Hadoop YARN UI, Spark Kullanıcı arabirimi ve Spark geçmiş sunucusunu kullanarak hata ayıklayın. Spark kümesi ile kullanılabilen bir not defteri kullanarak Spark işi başlatın, **Machine Learning: bir öngörülü İnceleme verilerinde MLLib kullanarak**tahmine dayalı analiz. Başka bir yaklaşımı kullanarak gönderdiğiniz bir uygulamayı (örneğin **Spark-gönder**) izlemek için aşağıdaki adımları kullanın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

* **[Machine Learning: MLLib kullanarak yiyecek İnceleme verilerinde tahmine dayalı analiz](apache-spark-machine-learning-mllib-ipython.md)** olan Not defterini çalıştırmaya başlamalısınız. Bu Not defterini çalıştırmaya ilişkin yönergeler için bağlantıyı izleyin.  

## <a name="track-an-application-in-the-yarn-ui"></a>YARN Kullanıcı arabiriminde uygulama izleme

1. YARN Kullanıcı arabirimini başlatın. **Küme panoları**altında **Yarn** ' yi seçin.

    ![Azure portal YARN Kullanıcı arabirimini başlatın](./media/apache-spark-job-debugging/launch-apache-yarn-ui.png)

   > [!TIP]  
   > Alternatif olarak, Ayrıca, ambarı kullanıcı arabiriminden YARN Kullanıcı arabirimini de başlatabilirsiniz. Ambarı Kullanıcı arabirimini başlatmak için **küme panoları**altında, **ambarı giriş** ' i seçin. Ambarı kullanıcı arabiriminden, etkin kaynak yöneticisi > **Kaynak Yöneticisi Kullanıcı arabirimine**> **Yarn** > **hızlı bağlantılar** ' a gidin.

2. Jupiter not defterlerini kullanarak Spark işini başlattığınız için, uygulamanın **remoteparlak mıknatıcs** (not defterlerinden başlatılan tüm uygulamaların adı) adına sahip olması gerekir. İş hakkında daha fazla bilgi edinmek için uygulama adına karşı uygulama KIMLIĞI ' ni seçin. Bu eylem uygulama görünümünü başlatır.

    ![Spark geçmiş sunucusu Spark uygulama KIMLIĞINI bul](./media/apache-spark-job-debugging/find-application-id1.png)

    Jupyter not defterlerinden başlatılan uygulamalar için, Not defterinizden çıkana kadar durum her zaman **çalışır** .

3. Uygulama görünümünden, uygulamayla ve günlüklerle (stdout/stderr) ilişkili kapsayıcıları bulmak için daha fazla ayrıntıya gidebilirsiniz. Spark Kullanıcı arabirimini aşağıda gösterildiği gibi **Izleme URL**'sine karşılık gelen bağlamaya tıklayarak da başlatabilirsiniz.

    ![Spark geçmiş sunucusu kapsayıcı günlüklerini indirme](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Spark Kullanıcı arabirimindeki bir uygulamayı izleme

Spark Kullanıcı arabiriminde, daha önce başlattığınız uygulama tarafından oluşturulan Spark işlerinin detayına gidebilirsiniz.

1. Spark Kullanıcı arabirimini başlatmak için, uygulama görünümünden, yukarıdaki ekran yakalama bölümünde gösterildiği gibi **izleme URL 'sine**karşı bağlantıyı seçin. Jupyter not defterinde çalışan uygulama tarafından başlatılan Spark işlerinin tümünü görebilirsiniz.

    ![Spark geçmiş sunucusu işleri sekmesi](./media/apache-spark-job-debugging/view-apache-spark-jobs.png)

2. Her bir yürütücü için işleme ve depolama bilgilerini görmek üzere **Yürüticileri** sekmesini seçin. Ayrıca, **Iş parçacığı döküm** bağlantısını seçerek çağrı yığınını da alabilirsiniz.

    ![Spark geçmiş sunucu yürüticileri sekmesi](./media/apache-spark-job-debugging/view-spark-executors.png)

3. Uygulamayla ilişkili aşamaları görmek için **aşamalar** sekmesini seçin.

    ![Spark geçmiş sunucusu aşamaları sekmesi](./media/apache-spark-job-debugging/view-apache-spark-stages.png "Spark aşamalarını görüntüle")

    Her aşamada, aşağıda gösterildiği gibi, yürütme istatistiklerini görüntüleyebileceğiniz birden fazla görev olabilir.

    ![Spark geçmiş sunucusu aşamaları sekme ayrıntıları](./media/apache-spark-job-debugging/view-spark-stages-details.png "Spark aşamaları ayrıntılarını görüntüle")

4. Aşama ayrıntıları sayfasında, DAG görselleştirmesini başlatabilirsiniz. Aşağıda gösterildiği gibi sayfanın üst kısmındaki **dag görselleştirme** bağlantısını genişletin.

    ![Spark aşamaları DAG görselleştirmesini görüntüle](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)

    DAG veya Direct Aclyic Graph, uygulamadaki farklı aşamaları temsil eder. Grafikteki her mavi kutu, uygulamadan çağrılan bir Spark işlemini temsil eder.

5. Aşama ayrıntıları sayfasında, uygulama zaman çizelgesi görünümünü de başlatabilirsiniz. Sayfanın üst kısmındaki **olay zaman çizelgesi** bağlantısını aşağıda gösterildiği gibi genişletin.

    ![Spark aşamaları olay zaman çizelgesini görüntüle](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)

    Bu görüntü, Spark olaylarını bir zaman çizelgesi biçiminde görüntüler. Zaman çizelgesi görünümü, işler arasında, iş içinde ve bir aşamada olmak üzere üç düzeyde kullanılabilir. Yukarıdaki görüntü, belirli bir aşama için zaman çizelgesi görünümünü yakalar.

   > [!TIP]  
   > **Yakınlaştırmayı Etkinleştir** onay kutusunu seçerseniz, zaman çizelgesi görünümünde sola ve sağa kaydırma yapabilirsiniz.

6. Spark Kullanıcı arabirimindeki diğer sekmeler Spark örneği hakkında da yararlı bilgiler sağlar.

   * Depolama sekmesi-uygulamanız bir RDD oluşturursa, depolama sekmesinden bilgileri bulabilirsiniz.
   * Ortam sekmesi-Bu sekme, Spark örneğiniz hakkında şu şekilde yararlı bilgiler sağlar:
     * Scala sürümü
     * Kümeyle ilişkili olay günlüğü dizini
     * Uygulamanın yürütücü çekirdekleri sayısı

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Spark geçmiş sunucusunu kullanarak tamamlanan işler hakkında bilgi bulma

Bir iş tamamlandıktan sonra, işle ilgili bilgiler Spark geçmiş sunucusunda kalıcıdır.

1. Spark geçmiş sunucusunu başlatmak için, **genel bakış** sayfasında, **küme panoları**altında **Spark geçmiş sunucusu** ' nu seçin.

    ![Spark geçmiş sunucusu Azure portal Başlat](./media/apache-spark-job-debugging/launch-spark-history-server.png "Spark geçmişini Başlat Sunucu1")

   > [!TIP]  
   > Alternatif olarak, aynı zamanda, ambarı kullanıcı arabiriminden Spark geçmiş sunucusu kullanıcı arabirimini de başlatabilirsiniz. Ambarı Kullanıcı arabirimini başlatmak için genel bakış dikey penceresinden, **küme panoları**altında, **ambarı giriş** ' i seçin. Ambarı kullanıcı arabiriminden **Spark2** > **hızlı bağlantılar** > **Spark2 geçmiş sunucusu kullanıcı arabirimi**' ne gidin.

2. Listelenen tüm tamamlanmış uygulamaları görürsünüz. Daha fazla bilgi için bir uygulamanın ayrıntısına gitmek üzere bir uygulama KIMLIĞI seçin.

    ![Spark geçmiş sunucusu tamamlanan uygulamalar](./media/apache-spark-job-debugging/view-completed-applications.png "Spark geçmişini Başlat Sunucu2")

## <a name="see-also"></a>Ayrıca bkz.

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Genişletilmiş Spark geçmiş sunucusunu kullanarak Apache Spark Işlerinde hata ayıklama](apache-azure-spark-history-server.md)
* [SSH aracılığıyla Azure Toolkit for IntelliJ ile Apache Spark uygulamalarda hata ayıklama](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
