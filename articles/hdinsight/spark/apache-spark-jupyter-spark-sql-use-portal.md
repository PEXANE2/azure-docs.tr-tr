---
title: "Hızlı başlangıç: Azure portal kullanarak HDInsight 'ta Spark kümesi oluşturma"
description: Bu hızlı başlangıçta, Azure HDInsight 'ta bir Apache Spark kümesi oluşturmak ve bir Spark SQL sorgusu çalıştırmak için Azure portal nasıl kullanılacağı gösterilmektedir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 09/27/2019
ms.custom: mvc
ms.openlocfilehash: 070f1f158ef6fb1d4b8f6a41f5b36d7cb16b24da
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77048512"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Azure HDInsight 'ta Apache Spark kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight 'ta bir Apache Spark kümesi oluşturmak için Azure portal kullanırsınız. Daha sonra bir Jupyter Not defteri oluşturun ve bunu, Apache Hive tablolarında Spark SQL sorguları çalıştırmak için kullanabilirsiniz. Azure HDInsight kuruluşlara yönelik, yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. HDInsight için Apache Spark Framework, bellek içi işleme kullanarak hızlı veri analizi ve küme bilgi işlem desteği sunar. Jupyter Not defteri verilerinizle etkileşim kurmanıza, kodu markı metniyle birleştirmenize ve basit görselleştirmeler yapmanıza olanak sağlar.

[Genel bakış: Azure HDInsight 'ta Apache Spark](apache-spark-overview.md) | [Apache Spark](https://spark.apache.org/) | [Apache Hive](https://hive.apache.org/) | [Jupyter Notebook](https://jupyter.org/)

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="create-an-apache-spark-cluster-in-hdinsight"></a>HDInsight 'ta Apache Spark kümesi oluşturma

Küme depolama birimi olarak Azure depolama Blobları kullanan bir HDInsight kümesi oluşturmak için Azure portal kullanın. Data Lake Storage Gen2'yi kullanma hakkında daha fazla bilgi için bkz. [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

> [!IMPORTANT]  
> İster kullanın, ister kullanmayın, HDInsight kümeleri faturalaması dakika başına eşit olarak dağıtılmıştır. Kullanmayı bitirdikten sonra kümenizi sildiğinizden emin olun. Daha fazla bilgi için bu makalenin [Kaynakları temizleme](#clean-up-resources) bölümüne bakın.

1. Azure portalda **Kaynak oluştur**’u seçin.

    ![Azure portal kaynak oluşturma](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-create.png "Azure portalında kaynak oluşturma")

1. **Yeni** sayfada **analiz** > **HDInsight**' ı seçin.

    ![HDInsight oluşturma Azure portal](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-create-hdinsight-spark-cluster.png "Azure portal HDInsight")

1. **Temel Bilgiler** bölümünde aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Abonelik  | Açılan listeden, bu küme için kullanılan bir Azure aboneliği seçin. Bu hızlı başlangıç için kullanılan abonelik **Azure**. |
    |Kaynak grubu | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Bu hızlı başlangıç için kullanılan kaynak grubu adı **Myresourcegroup**' dir. |
    |Küme adı | HDInsight kümenize bir ad verin. Bu hızlı başlangıç için kullanılan küme adı **myspark2019**.|
    |Konum   | Kaynak grubu için bir konum seçin. Şablon hem kümeyi oluşturmak için hem de varsayılan küme depolaması için bu konumu kullanır. Bu hızlı başlangıç için kullanılan konum **Doğu ABD**. |
    |Küme türü| Küme türü olarak **Spark** ' ı seçin.|
    |Küme sürümü|Bu alan, küme türü seçildikten sonra otomatik olarak varsayılan sürümle doldurulur.|
    |Küme oturum açma kullanıcı adı| Küme oturum açma kullanıcı adını girin.  Varsayılan ad *admin*' dir. Bu hesabı, hızlı başlangıçta daha sonra Jupyter not defterinde oturum açmak için kullanırsınız. |
    |Küme oturum açma parolası| Küme oturum açma parolasını girin. |
    |Secure Shell (SSH) kullanıcı adı| SSH kullanıcı adını girin. Bu hızlı başlangıç için kullanılan SSH kullanıcı adı, **sshuser** şeklindedir. Varsayılan olarak bu hesap, *Küme Oturum Açma kullanıcı adı* hesabıyla aynı parolayı paylaşır. |

    ![HDInsight kümesi temel yapılandırması oluşturma](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-basics-spark.png "HDInsight 'ta Spark kümesi oluşturma temel yapılandırma")

    Ileri ' yi seçin: **depolama sayfasına devam** etmek Için **> Depolama >** .

1. **Depolama** bölümünde aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü|Varsayılan **Azure Storage**değerini kullanın.|
    |Seçim yöntemi|Varsayılan değer **listesinden Seç ' i**kullanın.|
    |Birincil depolama hesabı|Otomatik doldurulmuş değeri kullanın.|
    |Kapsayıcı|Otomatik doldurulmuş değeri kullanın.|

    ![HDInsight kümesi temel yapılandırması oluşturma](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-storage.png "HDInsight 'ta Spark kümesi oluşturma temel yapılandırma")

    Devam etmek için **gözden geçir + oluştur** ' u seçin.

1. **Gözden geçir + oluştur**altında **Oluştur**' u seçin. Kümenin oluşturulması yaklaşık 20 dakika sürer. Sonraki oturumuna devam etmeden önce küme oluşturulması gerekir.

HDInsight kümeleri oluşturma ile ilgili bir sorun yaşıyorsanız, bunu yapmak için doğru izinlere sahip değilsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

Jupyter Notebook, çeşitli programlama dillerini destekleyen etkileşimli bir not defteri ortamıdır. Not defteri, verilerle etkileşim kurmanıza, kodu markdown metniyle birleştirmenize ve basit görselleştirmeler gerçekleştirmenize olanak sağlar.

1. [Azure portalını](https://portal.azure.com) açın.

1. **HDInsight kümeleri**’ni ve sonra oluşturduğunuz kümeyi seçin.

    ![Azure portalında HDInsight kümesini açma](./media/apache-spark-jupyter-spark-sql/azure-portal-open-hdinsight-cluster.png)

1. Portaldan **Küme panoları**’nı ve sonra **Jupyter Notebook**’u seçin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

   ![Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook açın](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook açın")

1. **Yeni** > **PySpark** seçeneklerini belirleyerek bir not defteri oluşturun.

   ![Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook oluşturma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook oluşturma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.

## <a name="run-apache-spark-sql-statements"></a>Apache Spark SQL deyimlerini Çalıştır

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve tanımlama için en çok kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

1. Çekirdeğin hazır olduğunu doğrulayın. Not defterinde çekirdek adının yanında boş bir daire görmeniz, çekirdeğin hazır olduğu anlamına gelir. Dolu daire, çekirdeğin meşgul olduğunu belirtir.

    ![HDInsight 'ta sorgu Apache Hive](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "HDInsight 'ta Hive sorgusu")

    Not defterini ilk kez başlattığınızda, çekirdek arka planda birkaç görev gerçekleştirir. Çekirdeğin hazır olmasını bekleyin.

1. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    HDInsight kümeniz ile bir Jupyter Notebook kullandığınızda, Spark SQL kullanarak Hive sorguları çalıştırmak için kullanabileceğiniz bir önceden ayarlanmış `sqlContext` alırsınız. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `sqlContext` ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sonuçları almak 30 saniye kadar sürer. Çıkış aşağıdakine benzer olacaktır:

    ![HDInsight 'ta sorgu Apache Hive](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "HDInsight 'ta Hive sorgusu")

    Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz.

1. `hivesampletable` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight 'ta Hive sorgusu çıkışı](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "HDInsight 'ta Hive sorgusu çıkışı")

1. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Not defterini kapatmak, küme kaynaklarını serbest bırakır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

HDInsight, verilerinizi Azure Depolama’da veya Azure Data Lake Storage’da depolar, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. [Sonraki adımlar](#next-steps) içinde listelenen öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalına geri dönüp **Sil**’i seçin.

![HDInsight kümesini silme Azure portal](./media/apache-spark-jupyter-spark-sql/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini Sil")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, HDInsight 'ta Apache Spark kümesi oluşturmayı ve temel Spark SQL sorgusunu çalıştırmayı öğrendiniz. Örnek verilerde etkileşimli sorgular çalıştırmak için bir HDInsight kümesi kullanmayı öğrenmek üzere bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)
