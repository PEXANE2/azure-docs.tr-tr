---
title: "Hızlı başlangıç: Azure portalLarını kullanarak HDInsight'ta Kıvılcım kümesi oluşturma"
description: Bu hızlı başlangıç, Azure HDInsight'ta bir Apache Spark kümesi oluşturmak ve bir Spark SQL sorgusu çalıştırmak için Azure portalının nasıl kullanılacağını gösterir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/25/2020
ms.openlocfilehash: 5f4d1b8ef742a8dcafa2b8e34a6209f85ae050d9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77650680"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak Azure HDInsight'ta Apache Spark kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight'ta bir Apache Spark kümesi oluşturmak için Azure portalını kullanırsınız. Daha sonra bir Jupyter dizüstü bilgisayar oluşturun ve Apache Hive tablolarına karşı Sql sorguları çalıştırmak için kullanabilirsiniz. Azure HDInsight kuruluşlara yönelik, yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. HDInsight için Apache Spark çerçevesi, bellek içi işleme yi kullanarak hızlı veri analitiği ve küme hesaplaması sağlar. Jupyter dizüstü bilgisayarınız verilerinizle etkileşimkurmanızı, kodu işaretleme metniyle birleştirmenizi ve basit görselleştirmeler yapmanızı sağlar.

Kullanılabilir yapılandırmaların ayrıntılı açıklamaları için [hdinsight'ta kümeler ayarlama](../hdinsight-hadoop-provision-linux-clusters.md)bölümüne bakın. Kümeoluşturmak için portalın kullanımı yla ilgili daha fazla bilgi için [bkz.](../hdinsight-hadoop-create-linux-clusters-portal.md)

> [!IMPORTANT]  
> İster kullanın, ister kullanmayın, HDInsight kümeleri faturalaması dakika başına eşit olarak dağıtılmıştır. Kullanmayı bitirdikten sonra kümenizi sildiğinizden emin olun. Daha fazla bilgi için bu makalenin [Kaynakları temizleme](#clean-up-resources) bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

## <a name="create-an-apache-spark-cluster-in-hdinsight"></a>HDInsight'ta Bir Apache Spark kümesi oluşturma

Küme depolama alanı olarak Azure Depolama Blobs kullanan bir HDInsight kümesi oluşturmak için Azure portalını kullanırsınız. Data Lake Storage Gen2'yi kullanma hakkında daha fazla bilgi için bkz. [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Üst menüden **+ Kaynak oluştur'u**seçin.

    ![Azure portalı bir kaynak oluşturur](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-create-resource.png "Azure portalında kaynak oluşturma")

1. **HDInsight oluştur küme** sayfasına gitmek için **Analytics** > **Azure HDInsight'ı** seçin.

1. Temel **Bilgiler** sekmesinden aşağıdaki bilgileri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Abonelik  | Açılan listeden, küme için kullanılan Azure aboneliğini seçin. |
    |Kaynak grubu | Açılan listeden, varolan kaynak grubunuzu seçin veya **yeni oluştur'u**seçin.|
    |Küme adı | Genel olarak benzersiz bir ad girin.|
    |Bölge   | Açılan listeden, kümenin oluşturulduğu bir bölge seçin. |
    |Küme türü| Liste açmak için küme türünü seçin'i seçin. Listeden **Kıvılcım'ı**seçin.|
    |Küme sürümü|Küme türü seçildikten sonra bu alan varsayılan sürümle otomatik olarak doldurulur.|
    |Küme oturum açma kullanıcı adı| Küme oturum açma kullanıcı adını girin.  Varsayılan adı **admin**. Daha sonra hızlı bir şekilde Jupyter not defterine giriş yapmak için bu hesabı kullanırsınız. |
    |Küme oturum açma parolası| Küme oturum açma parolasını girin. |
    |Secure Shell (SSH) kullanıcı adı| SSH kullanıcı adını girin. Bu hızlı başlangıç için kullanılan SSH kullanıcı adı, **sshuser** şeklindedir. Varsayılan olarak bu hesap, *Küme Oturum Açma kullanıcı adı* hesabıyla aynı parolayı paylaşır. |

    ![HDInsight küme temel yapılandırmaları oluşturma](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-basics-spark.png "HDInsight'ta Temel yapılandırmalarda Kıvılcım kümesi oluşturma")

    **Sonraki'ni seçin:** **Depolama** sayfasına devam etmek için Depolama >>.

1. **Depolama** bölümünde aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü|Varsayılan değeri kullanın **Azure Depolama**.|
    |Seçim yöntemi|Varsayılan değeri kullanın **Listeden seçin.**|
    |Birincil depolama hesabı|Otomatik doldurulan değeri kullanın.|
    |Kapsayıcı|Otomatik doldurulan değeri kullanın.|

    ![HDInsight küme temel yapılandırmaları oluşturma](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-storage.png "HDInsight'ta Temel yapılandırmalarda Kıvılcım kümesi oluşturma")

    Devam etmek için **Gözden Geçir + oluştur'u** seçin.

1. **İnceleme altında + oluştur**, **Oluştur'u**seçin. Kümenin oluşturulması yaklaşık 20 dakika sürer. Sonraki oturumuna devam etmeden önce küme oluşturulması gerekir.

HDInsight kümeleri oluşturma yla ilgili bir sorunla karşılaştıysanız, bunu yapmak için doğru izinlere sahip olmamuş olabilirsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

Jupyter Notebook, çeşitli programlama dillerini destekleyen etkileşimli bir not defteri ortamıdır. Not defteri, verilerle etkileşim kurmanıza, kodu markdown metniyle birleştirmenize ve basit görselleştirmeler gerçekleştirmenize olanak sağlar.

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/jupyter`kümenizin adı nerede' `CLUSTERNAME` ye gidin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

1. Not defteri oluşturmak için **Yeni** > **PySpark'ı** seçin.

   ![Etkileşimli Spark SQL sorgusunu çalıştırmak için bir Jupyter Notebook oluşturma](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusunu çalıştırmak için bir Jupyter Notebook oluşturma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.

## <a name="run-apache-spark-sql-statements"></a>Apache Spark SQL deyimlerini çalıştırın

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve tanımlama için en çok kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

1. Çekirdeğin hazır olduğunu doğrulayın. Not defterinde çekirdek adının yanında boş bir daire görmeniz, çekirdeğin hazır olduğu anlamına gelir. Dolu daire, çekirdeğin meşgul olduğunu belirtir.

    ![HDInsight'ta Apache Hive sorgusu](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "HDInsight'ta kovan sorgusu")

    Not defterini ilk kez başlattığınızda, çekirdek arka planda birkaç görev gerçekleştirir. Çekirdeğin hazır olmasını bekleyin.

1. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    HDInsight kümenize sahip bir Jupyter Notebook kullandığınızda, `sqlContext` Spark SQL kullanarak Hive sorgularını çalıştırmak için kullanabileceğiniz bir ön ayar alırsınız. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `sqlContext` ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sonuçları almak 30 saniye kadar sürer. Çıkış aşağıdakine benzer olacaktır:

    ![HDInsight'ta Apache Hive sorgusu](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-get-started-hive-query.png "HDInsight'ta kovan sorgusu")

    Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz.

1. `hivesampletable` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight'ta kovan sorgu çıktısı](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-get-started-hive-query-output.png "HDInsight'ta kovan sorgu çıktısı")

1. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Not defterini kapatmak, küme kaynaklarını serbest bırakır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

HDInsight verilerinizi Azure Depolama veya Azure Veri Gölü Depolama'ya kaydeder, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır. [Sonraki adımlar](#next-steps) içinde listelenen öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalına geri dönüp **Sil**’i seçin.

![Azure portalı bir HDInsight kümesini silme](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, HDInsight'ta bir Apache Spark kümesi oluşturmayı ve temel bir Spark SQL sorgusu çalıştırmayı öğrendiniz. Örnek verilerüzerinde etkileşimli sorguları çalıştırmak için HDInsight kümesini nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark'ta etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)
