---
title: 'Quickstart: Şablonu kullanarak Apache Spark kümesi oluşturma - Azure HDInsight'
description: Bu hızlı başlangıç, Azure HDInsight'ta Bir Apache Spark kümesi oluşturmak ve bir Spark SQL sorgusu çalıştırmak için Kaynak Yöneticisi şablonunun nasıl kullanılacağını gösterir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/13/2020
ms.openlocfilehash: a92e1410b688dc3117cf28fee2ba9231641b65bb
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81616864"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure HDInsight'ta Apache Spark kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight'ta [Bir Apache Spark](./apache-spark-overview.md) kümesi oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanırsınız. Daha sonra bir Jupyter dizüstü bilgisayar oluşturun ve Apache Hive tablolarına karşı Sql sorguları çalıştırmak için kullanabilirsiniz. Azure HDInsight kuruluşlara yönelik, yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. HDInsight için Apache Spark çerçevesi, bellek içi işleme yi kullanarak hızlı veri analitiği ve küme hesaplaması sağlar. Jupyter dizüstü bilgisayarınız verilerinizle etkileşimkurmanızı, kodu işaretleme metniyle birleştirmenizi ve basit görselleştirmeler yapmanızı sağlar.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-apache-spark-cluster"></a>Apache Spark kümesi oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-spark-linux)

:::code language="json" source="~/quickstart-templates/101-hdinsight-spark-linux/azuredeploy.json" range="1-143":::

Şablonda iki Azure kaynağı tanımlanır:

* [Microsoft.Storage/storageHesapları](https://docs.microsoft.com/azure/templates/microsoft.storage/storageaccounts): bir Azure Depolama Hesabı oluşturun.
* [Microsoft.HDInsight/cluster](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/clusters): bir HDInsight kümesi oluşturun.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure'da oturum açmak ve Kaynak Yöneticisi şablonunu açmak için aşağıdaki **Azure'a Dağıt** düğmesini seçin.

    [![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json)

1. Aşağıdaki değerleri yazın veya seçin:

    |Özellik |Açıklama |
    |---|---|
    |Abonelik|Açılan listeden, küme için kullanılan Azure aboneliğini seçin.|
    |Kaynak grubu|Açılan listeden, varolan kaynak grubunuzu seçin veya **yeni oluştur'u**seçin.|
    |Konum|Değer, kaynak grubu için kullanılan konumla otomatik olarak doldurulur.|
    |Küme Adı|Genel olarak benzersiz bir ad girin. Bu şablon için yalnızca küçük harfleri ve sayıları kullanın.|
    |Küme Oturum Açma Kullanıcı Adı|Kullanıcı adı sağlayın, varsayılan **yönetici**.|
    |Küme Oturum Açma Parolası|Bir parola sağlayın. Parola en az 10 karakter uzunluğunda olmalı ve en az bir basamak, bir büyük harf ve bir küçük harf, bir alfasayısal olmayan karakter içermelidir (karakterler hariç ' ' ' ). |
    |Ssh Kullanıcı Adı|Kullanıcı adı sağlayın, varsayılan **sshuser** olduğunu|
    |Ssh Şifre|Parolayı girin.|

    ![Azure Kaynak Yöneticisi şablonu kullanarak HDInsight'ta Kıvılcım kümesi oluşturma](./media/apache-spark-jupyter-spark-sql/resource-manager-template-spark.png "Azure Kaynak Yöneticisi şablonu kullanarak HDInsight'ta Kıvılcım kümesi oluşturma")

1. **ŞARTLARI VE KOŞULLARI**Gözden Geçirin. Sonra **ben yukarıda belirtilen hüküm ve koşulları kabul**seçin , sonra Satın **Al**. Dağıtımınızın devam ettiğine dair bir bildirim alırsınız. Bir küme oluşturmak yaklaşık 20 dakika sürer.

HDInsight kümeleri oluşturma yla ilgili bir sorunla karşılaştıysanız, bunu yapmak için doğru izinlere sahip olmamuş olabilirsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Küme oluşturulduktan sonra, **kaynak bağlantısına Git** ile **dağıtım başarılı** bildirimi alırsınız. Kaynak grubu sayfanız yeni HDInsight kümenizi ve kümeyle ilişkili varsayılan depolama alanını listeler. Her kümenin bir [Azure Depolama](../hdinsight-hadoop-use-blob-storage.md) hesabı veya Azure Veri Gölü [Depolama hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Kümeleri silmek depolama hesabını silmez.

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

[Jupyter Notebook,](https://jupyter.org/) çeşitli programlama dillerini destekleyen etkileşimli bir dizüstü bilgisayar ortamıdır. Not defteri, verilerinizle etkileşimkurmanızı, kodu işaretleme metniyle birleştirmenizi ve basit görselleştirmeler gerçekleştirmenizi sağlar.

1. Azure [portalını](https://portal.azure.com)açın.

2. **HDInsight kümeleri**’ni ve sonra oluşturduğunuz kümeyi seçin.

    ![Azure portalında HDInsight kümesini açın](./media/apache-spark-jupyter-spark-sql/azure-portal-open-hdinsight-cluster.png)

3. Portaldan, **Küme panoları** bölümünde **Jupyter Notebook'u**seçin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

   ![Etkileşimli Spark SQL sorgusunu çalıştırmak için Jupyter Notebook'u açın](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusunu çalıştırmak için Jupyter Notebook'u açın")

4. Not defteri oluşturmak için **Yeni** > **PySpark'ı** seçin.

   ![Etkileşimli Spark SQL sorgusunu çalıştırmak için bir Jupyter Notebook oluşturma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusunu çalıştırmak için bir Jupyter Notebook oluşturma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.

## <a name="run-apache-spark-sql-statements"></a>Apache Spark SQL deyimlerini çalıştırın

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve dönüştürme için en yaygın kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

1. Çekirdeğin hazır olduğunu doğrulayın. Not defterinde çekirdek adının yanında boş bir daire görmeniz, çekirdeğin hazır olduğu anlamına gelir. Dolu daire, çekirdeğin meşgul olduğunu belirtir.

    ![Çekirdek durumu](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "Çekirdek durumu")

    Not defterini ilk kez başlattığınızda, çekirdek arka planda birkaç görev gerçekleştirir. Çekirdeğin hazır olmasını bekleyin.

1. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```sql
    %%sql
    SHOW TABLES
    ```

    HDInsight kümenize sahip bir Jupyter Notebook kullandığınızda, `spark` Spark SQL kullanarak Kovan sorgularını çalıştırmak için kullanabileceğiniz önceden ayarlanmış bir oturum elde elabilirsiniz. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `spark` oturumu ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sorguyu ilk gönderdiğinde, Jupyter not defteri için bir Kıvılcım uygulaması oluşturur. Tamamlanması yaklaşık 30 saniye sürer. Spark uygulaması hazır olduğunda, sorgu yaklaşık bir saniye içinde yürütülür ve sonuçları üretir. Çıkış aşağıdakine benzer olacaktır:

    ![HDInsight'ta Apache Hive sorgusu](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "HDInsight'ta kovan sorgusu")

    Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz.

1. `hivesampletable` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight'ta kovan sorgu çıktısı](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "HDInsight'ta kovan sorgu çıktısı")

1. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Not defterinin kapatılması, Spark uygulaması da dahil olmak üzere küme kaynaklarını serbest bırakır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

Azure portalından kümenize gidin ve **Sil'i**seçin.

![Azure portalı bir HDInsight kümesini silme](./media/apache-spark-jupyter-spark-sql/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, HDInsight'ta bir Apache Spark kümesi oluşturmayı ve temel bir Spark SQL sorgusu çalıştırmayı öğrendiniz. Örnek verilerüzerinde etkileşimli sorguları çalıştırmak için HDInsight kümesini nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark'ta etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)
