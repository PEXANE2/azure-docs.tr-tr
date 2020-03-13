---
title: 'Hızlı başlangıç: şablon kullanarak Apache Spark kümesi oluşturma-Azure HDInsight'
description: Bu hızlı başlangıçta, Azure HDInsight 'ta bir Apache Spark kümesi oluşturmak ve bir Spark SQL sorgusu çalıştırmak için Kaynak Yöneticisi şablonunun nasıl kullanılacağı gösterilmektedir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/05/2020
ms.openlocfilehash: a4c207cdbe4bbd0fdef5e1da8da0f4b582702308
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79241209"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure HDInsight 'ta Apache Spark kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight 'ta bir Apache Spark kümesi oluşturmak için bir Azure Resource Manager şablonu kullanırsınız. Daha sonra bir Jupyter Not defteri oluşturun ve bunu, Apache Hive tablolarında Spark SQL sorguları çalıştırmak için kullanabilirsiniz. Azure HDInsight kuruluşlara yönelik, yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. HDInsight için Apache Spark Framework, bellek içi işleme kullanarak hızlı veri analizi ve küme bilgi işlem desteği sunar. Jupyter Not defteri verilerinizle etkileşim kurmanıza, kodu markı metniyle birleştirmenize ve basit görselleştirmeler yapmanıza olanak sağlar.

[Genel bakış: Azure HDInsight 'ta Apache Spark](apache-spark-overview.md) | [Apache Spark](https://spark.apache.org/) | [Apache Hive](https://hive.apache.org/) | [Jupyter Notebook](https://jupyter.org/) | [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Hdinsight&pageNumber=1&sort=Popular)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-apache-spark-cluster"></a>Apache Spark kümesi oluşturma

HDInsight 'ta bir Azure Resource Manager şablonu kullanarak Apache Spark kümesi oluşturun. Şablon [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/)' da bulunabilir. Kümenin JSON sözdizimi ve özellikleri için bkz. [Microsoft. HDInsight/kümeler](/azure/templates/microsoft.hdinsight/clusters).

Küme, küme depolama alanı olarak Azure Depolama Bloblarını kullanır. Data Lake Storage Gen2'yi kullanma hakkında daha fazla bilgi için bkz. [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

> [!IMPORTANT]  
> İster kullanın, ister kullanmayın, HDInsight kümeleri faturalaması dakika başına eşit olarak dağıtılmıştır. Kullanmayı bitirdikten sonra kümenizi sildiğinizden emin olun. Daha fazla bilgi için bu makalenin [Kaynakları temizleme](#clean-up-resources) bölümüne bakın.

1. Azure portalındaki şablonu yeni bir tarayıcı sekmesinde açmak için aşağıdaki bağlantıyı seçin:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank">Azure’a Dağıtma</a>

2. Aşağıdaki değerleri girin:

    | Özellik | Değer |
    |---|---|
    |Abonelik|Bu kümeyi oluşturmak için kullanılan Azure aboneliğinizi seçin. |
    | Kaynak grubu|Kaynak grubu oluşturun veya var olan bir grubu seçin. Kaynak grubu, projelerinize ait Azure kaynaklarını yönetmek için kullanılır. Bu hızlı başlangıç için kullanılan yeni kaynak grubu adı, **myspark20180403rg** şeklindedir.|
    | Konum|Kaynak grubu için bir konum seçin. Şablon, kümeyi oluşturmak için bu konumu ve varsayılan küme depolamayı kullanır. Bu hızlı başlangıç için kullanılan konum, **Doğu ABD 2**’dir.|
    | ClusterName|Oluşturmak istediğiniz küme için bir ad girin. Bu hızlı başlangıç için kullanılan yeni küme adı, **myspark20180403** şeklindedir.|
    | Küme oturum açma adı ve parolası|Varsayılan oturum açma adı admin ' dir. Küme oturumu açmak için bir parola seçin. Bu hızlı başlangıç için kullanılan oturum açma adı, admin’dir.|
    | SSH Kullanıcı adı ve parolası|SSH kullanıcısı için bir parola seçin. Bu hızlı başlangıç için kullanılan SSH kullanıcı adı, **sshuser** şeklindedir.|

    ![Azure Resource Manager şablonu kullanarak HDInsight 'ta Spark kümesi oluşturma](./media/apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Azure Resource Manager şablonu kullanarak HDInsight 'ta Spark kümesi oluşturma")

3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**' u seçin ve ardından **satın al**' ı seçin. Artık **Şablon Dağıtımını dağıtma** başlıklı yeni bir kutucuk görebilirsiniz. Kümenin oluşturulması yaklaşık 20 dakika sürer. Sonraki oturumuna devam etmeden önce küme oluşturulması gerekir.

HDInsight kümeleri oluşturma ile ilgili bir sorun yaşıyorsanız, bunu yapmak için doğru izinlere sahip değilsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="install-intellijeclipse-for-spark-applications"></a>Spark uygulamaları için IntelliJ/tutulma 'yi yükler

[Scala](https://www.scala-lang.org/)'Da yazılmış Spark uygulamaları geliştirmek için Azure Toolkit for IntelliJ/tutulma eklentisini kullanın ve ardından bunları doğrudan IntelliJ/tutulma tümleşik geliştirme ORTAMıNDAN (IDE) bir Azure HDInsight kümesine gönderebilirsiniz. Daha fazla bilgi için bkz. [Spark uygulaması yazmak/göndermek için IntelliJ kullanma](./apache-spark-intellij-tool-plugin.md) ve [Spark uygulaması yazmak/göndermek için Eclipse kullanma](./apache-spark-eclipse-tool-plugin.md).

## <a name="install-vscode-for-pysparkhive-applications"></a>PySpark/Hive uygulamaları için VSCode 'u yükler

Hive toplu işleri, etkileşimli Hive sorguları, PySpark toplu işi ve PySpark etkileşimli betikleri oluşturup göndermek için Visual Studio Code (VSCode) için Azure HDInsight Araçları’nın nasıl kullanılacağını öğrenin. Azure HDInsight Tools, VSCode tarafından desteklenen platformlara yüklenebilir. Bunlar arasında Windows, Linux ve MacOS yer alır. Daha fazla bilgi için bkz. [PySpark uygulaması yazmak/göndermek için VSCode kullanma](../hdinsight-for-vscode.md).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

[Jupyter Notebook](https://jupyter.org/) , çeşitli programlama dillerini destekleyen etkileşimli bir not defteri ortamıdır. Not defteri verilerinizle etkileşim kurmanıza, kodu markı metniyle birleştirmenize ve basit görselleştirmeler gerçekleştirmenize olanak tanır.

1. [Azure portalını](https://portal.azure.com) açın.

2. **HDInsight kümeleri**’ni ve sonra oluşturduğunuz kümeyi seçin.

    ![HDInsight kümesini Azure portal açın](./media/apache-spark-jupyter-spark-sql/azure-portal-open-hdinsight-cluster.png)

3. Portalda, **küme panoları** bölümünde **Jupyter Notebook**' yi seçin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

   ![Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook açın](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook açın")

4. **Yeni** > **PySpark** seçeneklerini belirleyerek bir not defteri oluşturun.

   ![Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook oluşturma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook oluşturma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.

## <a name="run-apache-spark-sql-statements"></a>Apache Spark SQL deyimlerini Çalıştır

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve dönüştürme için en yaygın kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

1. Çekirdeğin hazır olduğunu doğrulayın. Not defterinde çekirdek adının yanında boş bir daire görmeniz, çekirdeğin hazır olduğu anlamına gelir. Dolu daire, çekirdeğin meşgul olduğunu belirtir.

    ![Çekirdek durumu](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "Çekirdek durumu")

    Not defterini ilk kez başlattığınızda, çekirdek arka planda birkaç görev gerçekleştirir. Çekirdeğin hazır olmasını bekleyin.

1. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```sql
    %%sql
    SHOW TABLES
    ```

    HDInsight kümeniz ile bir Jupyter Notebook kullandığınızda, Spark SQL kullanarak Hive sorguları çalıştırmak için kullanabileceğiniz bir önceden ayarlanmış `spark` oturumu alırsınız. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `spark` oturumu ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sorguyu ilk kez gönderdiğinizde, Jupyter Not defteri için bir Spark uygulaması oluşturacaktır. Tamamlanması yaklaşık 30 saniye sürer. Spark uygulaması çalışmaya başladıktan sonra sorgu bir saniye içinde yürütülür ve sonuçları üretir. Çıkış aşağıdakine benzer olacaktır:

    ![HDInsight 'ta sorgu Apache Hive](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "HDInsight 'ta Hive sorgusu")

    Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz.

1. `hivesampletable` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight 'ta Hive sorgusu çıkışı](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "HDInsight 'ta Hive sorgusu çıkışı")

1. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Not defterini kapatmak Spark uygulaması da dahil olmak üzere küme kaynaklarını serbest bırakır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

HDInsight, verilerinizi ve jupi not defterlerinizi Azure Storage 'a veya Azure Data Lake Storage kaydeder, bu sayede bir kümeyi kullanımda olmadığında güvenle silebilirsiniz. Ayrıca, kullanımda olmasa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Kümenin ücretleri depolama ücretinden çok daha fazla olduğundan, kullanımda olmadıkları zaman kümeleri silmek ekonomik bir anlam sağlar. [Sonraki adımlar](#next-steps) içinde listelenen öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalına geri dönüp **Sil**’i seçin.

![HDInsight kümesini silme Azure portal](./media/apache-spark-jupyter-spark-sql/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini Sil")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, HDInsight 'ta Apache Spark kümesi oluşturmayı ve temel Spark SQL sorgusunu çalıştırmayı öğrendiniz. Örnek verilerde etkileşimli sorgular çalıştırmak için bir HDInsight kümesi kullanmayı öğrenmek üzere bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)
