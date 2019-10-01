---
title: "Hızlı başlangıç: Azure portal kullanarak HDInsight 'ta Spark kümesi oluşturma"
description: Bu hızlı başlangıçta, Azure HDInsight 'ta bir Apache Spark kümesi oluşturmak ve Spark SQL çalıştırmak için Azure portal nasıl kullanılacağı gösterilmektedir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 09/27/2019
ms.custom: mvc
ms.openlocfilehash: a4c7fe0d01bc9e5045cfe585c3f235636aa3dd22
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71676982"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Azure HDInsight 'ta Apache Spark kümesi oluşturma

Azure HDInsight 'ta Apache Spark kümesi oluşturmayı ve bkz. Spark SQL sorgularını Hive tablolarında çalıştırmayı öğrenin. Apache Spark, bellek içi işleme kullanarak hızlı veri analizi ve küme bilgi işlem işlemlerini mümkün. HDInsight 'ta Spark hakkında daha fazla bilgi için bkz. [genel bakış: Azure HDInsight 'ta Apache Spark](apache-spark-overview.md).

Bu hızlı başlangıçta, HDInsight Spark kümesi oluşturmak için Azure portal kullanırsınız. Küme, küme depolama alanı olarak Azure Storage Bloblarını kullanır. Data Lake Storage 2. kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: HDInsight 'ta kümeleri ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

> [!IMPORTANT]  
> HDInsight kümeleri için faturalandırma, bunları kullanıp kullanmayacağınızı de dakika başına eşit olarak dağıtılır. Kullanmayı bitirdikten sonra kümenizi sildiğinizden emin olun. Daha fazla bilgi için bu makalenin [Kaynakları Temizleme](#clean-up-resources) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="create-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesi oluşturma

1. Azure portal **kaynak oluştur** > **analiz** > **HDInsight**' ı seçin.

    (./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-create-hdinsight-spark-cluster.png "Azure Portal üzerinde") ![bir kaynak hdınsight HDInsight oluşturma Azure Portal]

1. **Temel bilgiler**bölümünde aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Aboneliğiniz  | Açılan listeden, bu küme için kullanılan bir Azure aboneliği seçin. Bu hızlı başlangıç için kullanılan abonelik **Azure**. |
    |Kaynak grubu | Yeni bir kaynak grubu oluşturmak mı yoksa mevcut bir kaynak grubu mı kullanmak istediğinizi belirtin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları tutan bir kapsayıcıdır. Bu hızlı başlangıç için kullanılan kaynak grubu adı **Myresourcegroup**' dir. |
    |Küme adı | HDInsight Spark kümenize bir ad verin. Bu hızlı başlangıç için kullanılan küme adı **myspark2019**.|
    |Konum   | Kaynak grubu için bir konum seçin. Şablon, kümeyi oluşturmak için bu konumu ve varsayılan küme depolama alanını kullanır. Bu hızlı başlangıç için kullanılan konum **Doğu ABD**. |
    |Küme türü| Küme türü olarak **Spark** ' ı seçin.|
    |Küme sürümü|Bu alan, küme türü seçildikten sonra otomatik olarak varsayılan sürümle doldurulur.|
    |Küme oturum açma Kullanıcı adı| Küme oturum açma Kullanıcı adı ' nı girin.  Varsayılan ad *admin*' dir. Bu hesabı, hızlı başlangıçta daha sonra Jupyter not defterinde oturum açmak için kullanırsınız. |
    |Küme oturum açma parolası| Küme oturum açma parolasını girin. |
    |Secure Shell (SSH) Kullanıcı adı| SSH kullanıcı adını girin. Bu hızlı başlangıç için kullanılan SSH Kullanıcı adı **sshuser**. Varsayılan olarak, bu hesap *küme oturum açma Kullanıcı adı* hesabıyla aynı parolayı paylaşır. |

    HDInsight ![Spark kümesi oluşturma temel yapılandırma](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-basics-spark.png "HDInsight 'ta Spark kümesi oluşturma temel yapılandırma")

    Ileri ' yi seçin: **depolama sayfasına devam** etmek Için **> Depolama >** .

1. **Depolama**bölümünde aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü|Varsayılan **Azure Storage**değerini kullanın.|
    |Seçim yöntemi|Varsayılan değer **listesinden Seç ' i**kullanın.|
    |Birincil depolama hesabı|Otomatik doldurulmuş değeri kullanın.|
    |Kapsayıcı|Otomatik doldurulmuş değeri kullanın.|

    HDInsight ![Spark kümesi oluşturma temel yapılandırma](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-storage-spark.png "HDInsight 'ta Spark kümesi oluşturma temel yapılandırma")

    Devam etmek için **gözden geçir + oluştur** ' u seçin.

1. **Gözden geçir + oluştur**altında **Oluştur**' u seçin. Kümenin oluşturulması yaklaşık 20 dakika sürer. Sonraki oturuma geçebilmeniz için önce kümenin oluşturulması gerekir.

HDInsight kümeleri oluştururken bir sorunla karşılaşırsanız, bunu yapmak için doğru izinlere sahip değilsiniz. Daha fazla bilgi için bkz. [erişim denetimi gereksinimleri](../hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="create-a-jupyter-notebook"></a>Jupyter Not defteri oluşturma

Jupyter Notebook, çeşitli programlama dillerini destekleyen etkileşimli bir not defteri ortamıdır. Not defteri verilerinizle etkileşim kurmanıza, kodu markı metniyle birleştirerek basit görselleştirmeler gerçekleştirmenize olanak tanır.

1. [Azure Portal](https://portal.azure.com)açın.

1. **HDInsight kümeleri**' ni seçin ve sonra oluşturduğunuz kümeyi seçin.

    ![HDInsight kümesini Azure portal açın](./media/apache-spark-jupyter-spark-sql/azure-portal-open-hdinsight-cluster.png)

1. Portaldan **küme panoları**' nı ve ardından **Jupyter Notebook**' yi seçin. İstenirse, küme için küme oturum açma kimlik bilgilerini girin.

   Etkileşimli Spark SQL sorgusu çalıştırmak için ![Jupyter Notebook aç](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "JUPYTER Notebook etkileşimli Spark SQL sorgusu çalıştırma")

1. Bir not defteri oluşturmak için **yeni** > **pyspark** öğesini seçin.

   ![Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook oluşturma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "etkileşimli Spark sql sorgusu çalıştırma Jupyter Notebook oluşturma")

   Yeni bir not defteri oluşturulur ve adsız (başlıksız. pynb) adıyla açılır.

## <a name="run-spark-sql-statements"></a>Spark SQL deyimlerini Çalıştır

SQL (Yapılandırılmış Sorgu Dili), verileri sorgulamak ve tanımlamak için en yaygın ve yaygın olarak kullanılan dildir. Tanıdık SQL söz dizimini kullanarak, yapılandırılmış verileri işlemek için Apache Spark bir uzantı olarak Spark SQL işlevleri.

1. Çekirdeğin hazırlanmaya yönelik olduğunu doğrulayın. Not defterinde çekirdek adının yanında boş bir daire gördüğünüzde çekirdek hazırlanmaya yönelik olur. Düz daire, çekirdeğin meşgul olduğunu gösterir.

    HDInsight 'ta ![HDInsight Spark1](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "Hive sorgusunda") sorgu Apache Hive Spark1

    Not defterini ilk kez başlattığınızda, çekirdek arka planda bazı görevleri gerçekleştirir. Çekirdeğin hazırlanmaya başlamasını bekleyin.

1. Aşağıdaki kodu boş bir hücreye yapıştırın ve ardından kodu çalıştırmak için **SHIFT + enter** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    HDInsight Spark kümeniz ile bir Jupyter Notebook kullandığınızda, Spark SQL kullanarak Hive sorguları çalıştırmak için kullanabileceğiniz, önceden ayarlanmış bir @no__t (0) alırsınız. `%%sql`, Hive sorgusunu çalıştırmak için önceden ayarlanmış `sqlContext` ' i kullanmayı Jupyter Notebook söyler. Sorgu, varsayılan olarak tüm HDInsight kümeleriyle birlikte gelen bir Hive tablosundan (**hivesampletable**) ilk 10 satırı alır. Sonuçların elde etmek yaklaşık 30 saniye sürer. Çıktı şöyle görünür:

    HDInsight 'ta ![HDInsight Spark2](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive sorgusunda") sorgu Apache Hive Spark2

    Jupyter 'da bir sorguyu her çalıştırdığınızda, Web tarayıcısı penceresi başlığında Not defteri başlığı ile birlikte **(meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **Pyspark** metninin yanında bir düz daire görürsünüz.

1. @No__t-0 ' da verileri görmek için başka bir sorgu çalıştırın.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    Sorgu çıkışını göstermek için ekran yenilenir.

    HDInsight Spark(./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive sorgu çıkışında HDInsight") Spark 'ta ![Hive sorgusu çıkışı]

1. Not defterindeki **Dosya** menüsünde **Kapat ve Durdur**' u seçin. Not defterini kapatmak küme kaynaklarını serbest bırakır.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

HDInsight, verileri Azure depolama alanına veya Azure Data Lake Storage kaydederek bir kümeyi kullanımda olmadığında güvenle silebilirsiniz. Ayrıca, kullanımda olmadığı halde bir HDInsight kümesi için de ücretlendirilirsiniz. Kümenin ücretleri depolama ücretinden çok daha fazla olduğundan, kullanımda olmadıkları zaman kümeleri silmek ekonomik bir anlam sağlar. [Sonraki adımlarda](#next-steps) hemen listelenen öğreticide çalışmayı planlıyorsanız kümeyi saklamak isteyebilirsiniz.

Azure portal geri dönün ve **Sil**' i seçin.

![HDInsight kümesini silme Azure Portal](./media/apache-spark-jupyter-spark-sql/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını da seçerek kaynak grubu sayfasını açabilir ve **kaynak grubunu sil**' i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, HDInsight Spark kümesi oluşturmayı ve temel Spark SQL sorgusunu çalıştırmayı öğrendiniz. Örnek verilerde etkileşimli sorgular çalıştırmak için bir HDInsight Spark kümesini nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)
