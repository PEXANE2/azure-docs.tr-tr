---
title: "Hızlı başlangıç: Azure portal kullanarak HDInsight 'ta Apache Hadoop ve Apache Hive"
description: Bu hızlı başlangıçta, HDInsight Hadoop kümesi oluşturmak için Azure portal kullanırsınız
keywords: Hadoop kullanmaya başlama, Hadoop Linux, Hadoop hızlı başlangıç, Hive Başlarken, Hive hızlı başlangıç
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.topic: quickstart
ms.date: 09/25/2019
ms.openlocfilehash: 5d87cc7fdcd9c8065c2a9886b970b406df0d8fc8
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677924"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Azure HDInsight 'ta Apache Hadoop kümesi oluşturma

Bu makalede, HDInsight 'ta Azure portal kullanarak [Apache Hadoop](https://hadoop.apache.org/) kümeleri oluşturmayı ve sonra hdınsight 'ta Apache Hive işleri çalıştırmayı öğreneceksiniz. Hadoop işlerinin çoğu Batch işlerdir. Bir küme oluşturur, bazı işleri çalıştırır ve ardından kümeyi silersiniz. Bu makalede, üç görevi de gerçekleştirirsiniz.

Bu hızlı başlangıçta, HDInsight Hadoop kümesi oluşturmak için Azure portal kullanırsınız. [Azure Resource Manager şablonunu](apache-hadoop-linux-tutorial-get-started.md)kullanarak da bir küme oluşturabilirsiniz.

HDInsight Şu anda [yedi farklı küme türüyle](../hdinsight-overview.md#cluster-types-in-hdinsight)birlikte gelir. Her küme türü farklı bir bileşen kümesini destekler. Tüm küme türleri Hive 'yi destekler. HDInsight 'ta desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sunulan Apache Hadoop kümesi sürümlerindeki yenilikler nelerdir?](../hdinsight-component-versioning.md)  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="create-an-apache-hadoop-cluster"></a>Apache Hadoop kümesi oluşturma

Bu bölümde, Azure portal kullanarak HDInsight 'ta bir Hadoop kümesi oluşturacaksınız.

1. [Azure Portal](https://portal.azure.com)oturum açın.

1. Azure portal, **kaynak oluşturma** > **analiz** > **HDInsight**' a gidin.

    ![Kaynak HDInsight kümesi oluşturma](./media/apache-hadoop-linux-create-cluster-get-started-portal/create-hdinsight-cluster.png "kaynak HDInsight kümesi oluşturma")

1. **Temel bilgiler**altında, aşağıdaki değerleri girin veya seçin:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Aboneliğiniz    |  Azure aboneliğinizi seçin. |
    |Kaynak grubu     | Bir kaynak grubu oluşturun veya var olan bir kaynak grubunu seçin.  Kaynak grubu, Azure bileşenlerinin bir kapsayıcısıdır.  Bu durumda, kaynak grubu HDInsight kümesini ve bağımlı Azure Depolama hesabını içerir. |
    |Küme adı   | Hadoop kümesi için bir ad girin. HDInsight 'taki tüm kümeler aynı DNS ad alanını paylaştığından, bu ad bu adın benzersiz olması gerekir. Ad, harf, sayı ve kısa çizgi gibi en fazla 59 karakter içerebilir. Adın ilk ve son karakterleri kısa çizgi olamaz. |
    |Konum    | Kümenizi oluşturmak istediğiniz Azure konumunu seçin.  Daha iyi performans için size daha yakın bir konum seçin. |
    |Küme türü| **Küme türünü seç**' i seçin. Ardından küme türü olarak **Hadoop** ' ı seçin.|
    |Version|Küme türü için varsayılan sürüm belirtilecektir. Farklı bir sürüm belirtmek istiyorsanız, açılan listeden seçim yapın.|
    |Küme oturum açma Kullanıcı adı ve parolası    | Varsayılan oturum açma adı **admin**' dir. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, bir büyük harf ve bir küçük harf, bir alfasayısal olmayan karakter (' "' \) karakterleri dışında) içermelidir. "@No__t-1" gibi ortak parolalar **sağlamadığınızdan** emin olun.|
    |Secure Shell (SSH) Kullanıcı adı | Varsayılan Kullanıcı adı **sshuser**' dır.  SSH Kullanıcı adı için başka bir ad sağlayabilirsiniz. |
    |SSH için küme oturum açma parolasını kullanma| SSH kullanıcısı için, küme oturum açma kullanıcısı için sağladınız aynı parolayı kullanmak üzere bu onay kutusunu işaretleyin.|

    ![HDInsight Linux Başlarken küme temel değerleri],(./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-basics-blank.png "HDInsight kümesi oluşturmak için temel değerler sağlar")

    **Sonraki: depolama > >** depolama ayarlarına ilerlemek için seçin.

1. **Depolama** sekmesinden aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü|Varsayılan **Azure Storage**değerini kullanın.|
    |Seçim yöntemi|Varsayılan değer **listesinden Seç ' i**kullanın.|
    |Birincil depolama hesabı|Açılan listeyi kullanarak mevcut bir depolama hesabını seçin veya **Yeni oluştur**' u seçin. Yeni bir hesap oluşturursanız, ad 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içerebilir|
    |Kapsayıcı|Oto doldurulmuş değeri kullanın.|

    ![HDInsight Linux kullanmaya başlama küme depolama değerleri],(./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-storage-blank.png "HDInsight kümesi oluşturmak için depolama değerleri") sağlar

    **Gözden geçir + oluştur** sekmesini seçin.

1. **Gözden geçir + oluştur** sekmesinden, önceki adımlarda seçtiğiniz değerleri doğrulayın.

    ![HDInsight Linux kullanmaya başlama küme Özeti](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-review-create-hadoop.png "HDInsight Linux kullanmaya başlama küme Özeti")

1. **Oluştur**' u seçin. Bir küme oluşturmak yaklaşık 20 dakika sürer.

Küme oluşturulduktan sonra, Azure portal kümeye genel bakış sayfasını görürsünüz.

![HDInsight Linux kullanmaya başlama küme ayarları](./media/apache-hadoop-linux-create-cluster-get-started-portal/cluster-settings-overview.png "HDInsight küme özellikleri")

Her kümenin bir [Azure depolama hesabı](../hdinsight-hadoop-use-blob-storage.md) veya [Azure Data Lake hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Küme silme, depolama hesabını silmez.

> [!NOTE]  
> Diğer küme oluşturma yöntemleri ve bu hızlı başlangıçta kullanılan özellikleri anlamak için bkz. [HDInsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="run-apache-hive-queries"></a>Apache Hive sorguları çalıştırma

[Apache Hive](hdinsight-use-hive.md) , HDInsight 'ta kullanılan en popüler bileşendir. HDInsight 'ta Hive işlerini çalıştırmanın birçok yolu vardır. Bu hızlı başlangıçta portaldan ambarı Hive görünümünü kullanırsınız. Hive işlerini göndermeye yönelik diğer yöntemler için bkz. [HDInsight 'Ta Hive kullanma](hdinsight-use-hive.md).

1. Ambarı açmak için, önceki ekran görüntüsünden **küme panosu**' nu seçin.  @No__t-0 ' a da gidebilirsiniz. burada, önceki bölümde oluşturduğunuz küme `ClusterName` ' dir.

    ![HDInsight Linux kullanmaya başlama küme panosu](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-open-cluster-dashboard.png "HDInsight Linux kullanmaya başlama küme panosu")

2. Kümeyi oluştururken belirttiğiniz Hadoop Kullanıcı adını ve parolasını girin. Varsayılan Kullanıcı adı **admin**' dir.

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Hive görünümünü** açın:

    Ambarı(./media/apache-hadoop-linux-create-cluster-get-started-portal/hdi-select-hive-view.png "HDInsight Hive Görüntüleyicisi menüsünden") ![Hive görünümü seçme]

4. **Sorgu** sekmesinde, aşağıdaki HiveQL deyimlerini çalışma sayfasına yapıştırın:

    ```sql
    SHOW TABLES;
    ```

    ![HDInsight Hive görünümü sorgu Düzenleyicisi](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdi-apache-hive-view1.png "HDInsight Hive görünümü sorgu Düzenleyicisi")

5. **Yürüt**' ü seçin. **Sorgu** sekmesinin altında bir **sonuçlar** sekmesi görünür ve işle ilgili bilgileri görüntüler. 

    Sorgu tamamlandıktan sonra **sorgu** sekmesi işlemin sonuçlarını görüntüler. **Hivesampletable**adlı bir tablo görürsünüz. Bu örnek Hive tablosu tüm HDInsight kümeleriyle birlikte gelir.

    ![Hdınsight Apache Hive sonuçları]görüntüleme HDInsight(./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-hive-views.png "Apache Hive") görüntüleme sonuçları

6. Aşağıdaki sorguyu çalıştırmak için 4. ve 5. adımı yineleyin:

    ```sql
    SELECT * FROM hivesampletable;
    ```

7. Sorgunun sonuçlarını da kaydedebilirsiniz. Sağdaki menü düğmesini seçin ve sonuçları bir CSV dosyası olarak indirmek mi yoksa kümeyle ilişkili depolama hesabına mi depolamak istediğinizi belirtin.

    ![Apache Hive sorgusunun sonucunu kaydetme](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-hive-view-save-results.png "Apache Hive sorgusunun sonucunu kaydetme")

Bir Hive işini tamamladıktan sonra, [sonuçları Azure SQL veritabanı 'na veya SQL Server veritabanına aktarabilirsiniz](apache-hadoop-use-sqoop-mac-linux.md), ayrıca [Excel kullanarak sonuçları görselleştirebilirsiniz](apache-hadoop-connect-excel-power-query.md). HDInsight 'ta Hive kullanma hakkında daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop ile Apache Hive ve HiveQL kullanarak örnek Apache Log4J dosyası çözümleme](hdinsight-use-hive.md).

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile Verileriniz Azure Storage 'da depolanır, bu sayede bir kümeyi kullanımda olmadığında güvenle silebilirsiniz. Ayrıca, kullanımda olmadığı halde bir HDInsight kümesi için de ücretlendirilirsiniz. Kümenin ücretleri depolama ücretinden çok daha fazla olduğundan, kullanımda olmadıkları zaman kümeleri silmek ekonomik bir anlam sağlar.

> [!NOTE]  
> HDInsight 'ta Hadoop kullanarak ETL işlemlerini nasıl çalıştıracağınızı öğrenmek için *hemen* bir sonraki makaleye devam ediyorsanız, kümeyi çalışır durumda tutmak isteyebilirsiniz. Bunun nedeni öğreticide bir Hadoop kümesi oluşturmanız gerekir. Ancak, bir sonraki makaleyi hemen ilerleyecekseniz kümeyi şimdi silmeniz gerekir.

### <a name="to-delete-the-cluster-andor-the-default-storage-account"></a>Kümeyi ve/veya varsayılan depolama hesabını silmek için

1. Azure portal sahip olduğunuz tarayıcı sekmesine geri dönün. Kümeye Genel Bakış sayfasında olursunuz. Yalnızca kümeyi silmek ve varsayılan depolama hesabını sürdürmek istiyorsanız **Sil**' i seçin.

    ![Azure HDInsight kümeyi silme](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-delete-cluster.png "Azure HDInsight kümesini silme")

2. Kümeyi ve varsayılan depolama hesabını silmek istiyorsanız, kaynak grubu adını (önceki ekran görüntüsünde vurgulanan) seçerek kaynak grubu sayfasını açın.

3. Kümeyi ve varsayılan depolama hesabını içeren kaynak grubunu silmek için **kaynak grubunu sil** ' i seçin. Bu kaynak grubu silindiğinde depolama hesabı silinir. Depolama hesabını korumak istiyorsanız, yalnızca kümeyi silmeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kaynak Yöneticisi şablonu kullanarak Linux tabanlı HDInsight kümesi oluşturmayı ve temel Hive sorguları gerçekleştirmeyi öğrendiniz. Sonraki makalede, HDInsight 'ta Hadoop kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemini nasıl gerçekleştireceğinizi öğreneceksiniz.

> [!div class="nextstepaction"]
> [HDInsight üzerinde etkileşimli sorgu kullanarak verileri ayıklama, dönüştürme ve yükleme](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
