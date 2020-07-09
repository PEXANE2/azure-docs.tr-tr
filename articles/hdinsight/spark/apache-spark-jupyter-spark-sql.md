---
title: 'Hızlı başlangıç: şablon kullanarak Apache Spark kümesi oluşturma-Azure HDInsight'
description: Bu hızlı başlangıçta, Azure HDInsight 'ta bir Apache Spark kümesi oluşturmak ve bir Spark SQL sorgusu çalıştırmak için Kaynak Yöneticisi şablonunun nasıl kullanılacağı gösterilmektedir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/13/2020
ms.openlocfilehash: b5f3f0af39830a25680b5dd7c8c47a5848fe5986
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86084826"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-arm-template"></a>Hızlı başlangıç: ARM şablonunu kullanarak Azure HDInsight 'ta Apache Spark kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight 'ta bir [Apache Spark](./apache-spark-overview.md) kümesi oluşturmak için bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız. Daha sonra bir Jupyter Not defteri oluşturun ve bunu, Apache Hive tablolarında Spark SQL sorguları çalıştırmak için kullanabilirsiniz. Azure HDInsight kuruluşlara yönelik, yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. HDInsight için Apache Spark Framework, bellek içi işleme kullanarak hızlı veri analizi ve küme bilgi işlem desteği sunar. Jupyter Not defteri verilerinizle etkileşim kurmanıza, kodu markı metniyle birleştirmenize ve basit görselleştirmeler yapmanıza olanak sağlar.

Birden çok kümeyi birlikte kullanıyorsanız, bir sanal ağ oluşturmak isteyeceksiniz ve bir Spark kümesi kullanıyorsanız, Hive ambarı bağlayıcısını de kullanmak isteyeceksiniz. Daha fazla bilgi için bkz. [Azure HDInsight için bir sanal ağ planlayın](../hdinsight-plan-virtual-network-deployment.md) ve [Hive ambarı Bağlayıcısı ile Apache Spark ve Apache Hive tümleştirin](../interactive-query/apache-hive-warehouse-connector.md).

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure 'A dağıt** düğmesini seçin. Şablon Azure portal açılır.

[![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json)

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux).

:::code language="json" source="~/quickstart-templates/101-hdinsight-spark-linux/azuredeploy.json" range="1-143" highlight="51-85":::

Şablonda iki Azure kaynağı tanımlanmıştır:

* [Microsoft. Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts): bir Azure depolama hesabı oluşturun.
* [Microsoft. HDInsight/Cluster](/azure/templates/microsoft.hdinsight/clusters): HDInsight kümesi oluşturma.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure 'da oturum açmak ve ARM şablonunu açmak için aşağıdaki **Azure 'A dağıt** düğmesini seçin.

    [![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json)

1. Aşağıdaki değerleri yazın veya seçin:

    |Özellik |Açıklama |
    |---|---|
    |Abonelik|Aşağı açılan listeden, küme için kullanılan Azure aboneliğini seçin.|
    |Kaynak grubu|Aşağı açılan listeden, mevcut kaynak grubunuzu seçin veya **Yeni oluştur**' u seçin.|
    |Konum|Değer, kaynak grubu için kullanılan konum ile tekrar doldurulur.|
    |Küme Adı|Genel olarak benzersiz bir ad girin. Bu şablon için yalnızca küçük harfler ve rakamlar kullanın.|
    |Küme Oturum Açma Kullanıcı Adı|Kullanıcı adını belirtin, varsayılan olarak **admin**' dir.|
    |Küme Oturum Açma Parolası|Bir parola belirtin. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, bir büyük harf ve bir küçük harf, bir alfasayısal olmayan karakter (' "' karakterleri dışında) içermelidir. |
    |SSH Kullanıcı adı|Kullanıcı adını belirtin, varsayılan olarak **sshuser**|
    |SSH parolası|Parolayı girin.|

    ![Azure Resource Manager şablonu kullanarak HDInsight 'ta Spark kümesi oluşturma](./media/apache-spark-jupyter-spark-sql/resource-manager-template-spark.png "Azure Resource Manager şablonu kullanarak HDInsight 'ta Spark kümesi oluşturma")

1. **Hüküm ve koşulları**gözden geçirin. Ardından, **yukarıda belirtilen hüküm ve koşulları kabul ediyorum**' u ve ardından **satın al**' ı seçin. Dağıtımınızın devam ettiğini belirten bir bildirim alırsınız. Bir küme oluşturmak yaklaşık 20 dakika sürer.

HDInsight kümeleri oluşturma ile ilgili bir sorun yaşıyorsanız, bunu yapmak için doğru izinlere sahip değilsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Küme oluşturulduktan sonra **Kaynağa Git** bağlantısına sahip bir **dağıtım başarılı** bildirimi alırsınız. Kaynak grubu sayfanız yeni HDInsight kümenizi ve kümeyle ilişkili varsayılan depolamayı listeler. Her kümenin bir [Azure depolama](../hdinsight-hadoop-use-blob-storage.md) hesabı veya [Azure Data Lake Storage hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Bu, varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Küme silme, depolama hesabını silmez.

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

[Jupyter Notebook](https://jupyter.org/) , çeşitli programlama dillerini destekleyen etkileşimli bir not defteri ortamıdır. Not defteri verilerinizle etkileşim kurmanıza, kodu markı metniyle birleştirmenize ve basit görselleştirmeler gerçekleştirmenize olanak tanır.

1. [Azure Portal](https://portal.azure.com)açın.

2. **HDInsight kümeleri**’ni ve sonra oluşturduğunuz kümeyi seçin.

    ![HDInsight kümesini Azure portal açın](./media/apache-spark-jupyter-spark-sql/azure-portal-open-hdinsight-cluster.png)

3. Portalda, **küme panoları** bölümünde **Jupyter Notebook**' yi seçin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

   ![Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook açın](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusu çalıştırmak için Jupyter Notebook açın")

4. **New**  >  Bir not defteri oluşturmak için yeni**pyspark** ' ı seçin.

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

    HDInsight kümeniz ile bir Jupyter Notebook kullandığınızda, `spark` Spark SQL kullanarak Hive sorguları çalıştırmak için kullanabileceğiniz bir önceden ayarlanmış oturum alırsınız. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `spark` oturumu ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sorguyu ilk kez gönderdiğinizde, Jupyter Not defteri için bir Spark uygulaması oluşturacaktır. Tamamlanması yaklaşık 30 saniye sürer. Spark uygulaması çalışmaya başladıktan sonra sorgu bir saniye içinde yürütülür ve sonuçları üretir. Çıkış aşağıdakine benzer olacaktır:

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

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile Verileriniz Azure Storage 'da depolanır, bu sayede bir kümeyi kullanımda olmadığında güvenle silebilirsiniz. Ayrıca, kullanımda olmasa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Kümenin ücretleri depolama ücretinden çok daha fazla olduğundan, kullanımda olmadıkları zaman kümeleri silmek ekonomik bir anlam sağlar.

Azure portal kümenize gidin ve **Sil**' i seçin.

![HDInsight kümesini silme Azure portal](./media/apache-spark-jupyter-spark-sql/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini Sil")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, HDInsight 'ta Apache Spark kümesi oluşturmayı ve temel Spark SQL sorgusunu çalıştırmayı öğrendiniz. Örnek verilerde etkileşimli sorgular çalıştırmak için bir HDInsight kümesi kullanmayı öğrenmek üzere bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)
