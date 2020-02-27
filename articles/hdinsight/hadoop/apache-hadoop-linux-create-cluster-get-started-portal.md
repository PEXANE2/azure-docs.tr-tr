---
title: 'Hızlı başlangıç: Apache Hadoop, Apache Hive & Azure HDInsight portalı'
description: Bu hızlı başlangıçta, HDInsight Hadoop kümesi oluşturmak için Azure portal kullanırsınız
keywords: hadoop kullanmaya başlama,hadoop linux,hadoop hızlı başlangıç,hive kullanmaya başlama,hive hızlı başlangıç
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.date: 02/24/2020
ms.openlocfilehash: 7d2dd65224aad231db9574aa5a1fa4a00b328da8
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623552"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Azure HDInsight 'ta Apache Hadoop kümesi oluşturma

Bu makalede, HDInsight 'ta Azure portal kullanarak Apache Hadoop kümeleri oluşturmayı ve sonra HDInsight 'ta Apache Hive işleri çalıştırmayı öğreneceksiniz. Hadoop işlerinin çoğu toplu işlemdir. Bir küme oluşturur, bazı işleri çalıştırır ve kümeyi silersiniz. Bu makalede, üç görevi de gerçekleştirirsiniz. Kullanılabilir yapılandırmaların derinlemesine açıklamaları için bkz. [HDInsight 'ta kümeleri ayarlama](../hdinsight-hadoop-provision-linux-clusters.md). Küme oluşturmak üzere portalın kullanımıyla ilgili daha fazla bilgi için bkz. [portalda kümeler oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md).

Bu hızlı başlangıçta, HDInsight Hadoop kümesi oluşturmak için Azure portalını kullanırsınız. [Azure Resource Manager şablonunu](apache-hadoop-linux-tutorial-get-started.md) kullanarak da küme oluşturabilirsiniz.

HDInsight Şu anda [yedi farklı küme türüyle](../hdinsight-overview.md#cluster-types-in-hdinsight)birlikte gelir. Her küme türü farklı bir bileşen kümesini destekler. Tüm küme türleri Hive'ı destekler. HDInsight 'ta desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sunulan Apache Hadoop kümesi sürümlerindeki yenilikler nelerdir?](../hdinsight-component-versioning.md)  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-an-apache-hadoop-cluster"></a>Apache Hadoop kümesi oluşturma

Bu bölümde, Azure portalını kullanarak HDInsight’ta Hadoop kümesi oluşturursunuz.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Üstteki menüden **+ kaynak oluştur**' u seçin.

    ![Kaynak HDInsight kümesi oluşturma](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-create-resource.png "Kaynak HDInsight kümesi oluşturma")

1. **HDInsight kümesi oluşturma** sayfasına gitmek için **analiz** > **Azure HDInsight** ' ı seçin.

1. **Temel bilgiler** sekmesinde, aşağıdaki bilgileri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Abonelik    |  Aşağı açılan listeden, küme için kullanılan Azure aboneliğini seçin. |
    |Kaynak grubu     | Aşağı açılan listeden, mevcut kaynak grubunuzu seçin veya **Yeni oluştur**' u seçin.|
    |Küme adı   | Genel olarak benzersiz bir ad girin. Ad, harf, sayı ve kısa çizgi gibi en fazla 59 karakter içerebilir. Adın ilk ve son karakterleri kısa çizgi olamaz. |
    |Bölge    | Aşağı açılan listeden, kümenin oluşturulduğu bir bölge seçin.  Daha iyi performans için kendinize yakın bir konum seçin. |
    |Küme türü| **Küme türünü seç**' i seçin. Ardından küme türü olarak **Hadoop** ' ı seçin.|
    |Sürüm|Aşağı açılan listeden bir **Sürüm**seçin. Ne seçeceğimizi bilmiyorsanız varsayılan sürümü kullanın.|
    |Küme oturum açma Kullanıcı adı ve parolası    | Varsayılan oturum açma adı **admin**' dir. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, bir büyük harf ve bir küçük harf, bir alfasayısal olmayan karakter (' "\)' karakterleri dışında) içermelidir. " **" gibi genel parolalar** sağlamadığınızdanPass@word1 emin olun.|
    |Secure Shell (SSH) kullanıcı adı | Varsayılan kullanıcı adı **sshuser** şeklindedir.  SSH kullanıcı adı için başka bir ad sağlayabilirsiniz. |
    |SSH için küme oturum açma parolasını kullanma| SSH kullanıcısı için, küme oturum açma kullanıcısı için sağladınız aynı parolayı kullanmak üzere bu onay kutusunu işaretleyin.|

    ![HDInsight Linux kullanmaya başlama küme temel değerlerini sağlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-basics-blank.png "HDInsight kümesi oluşturmak için temel değerler sağlama")

    **Sonraki: depolama > >** depolama ayarlarına ilerlemek için seçin.

1. **Depolama** sekmesinden aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü|Varsayılan **Azure Storage**değerini kullanın.|
    |Seçim yöntemi|Varsayılan değer **listesinden Seç ' i**kullanın.|
    |Birincil depolama hesabı|Açılan listeyi kullanarak mevcut bir depolama hesabını seçin veya **Yeni oluştur**' u seçin. Yeni bir hesap oluşturursanız, ad 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içerebilir|
    |Kapsayıcı|Oto doldurulmuş değeri kullanın.|

    ![HDInsight Linux kullanmaya başlama küme depolama değerlerini sağlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-storage.png "HDInsight kümesi oluşturmak için depolama değerleri sağlama")

    Her kümenin bir [Azure Depolama hesabı](../hdinsight-hadoop-use-blob-storage.md) veya [Azure Data Lake hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Bu, varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Küme silme, depolama hesabını silmez.

    **Gözden geçir + oluştur** sekmesini seçin.

1. **Gözden geçir + oluştur** sekmesinden, önceki adımlarda seçtiğiniz değerleri doğrulayın.

    ![HDInsight Linux kullanmaya başlama kümesi Özeti](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-review-create-hadoop.png "HDInsight Linux kullanmaya başlama kümesi Özeti")

1. **Oluştur**’u seçin. Bir küme oluşturmak yaklaşık 20 dakika sürer.

    Küme oluşturulduktan sonra, Azure portalında kümeye genel bakış sayfasını görürsünüz.

    ![HDInsight Linux kullanmaya başlama kümesi ayarları](./media/apache-hadoop-linux-create-cluster-get-started-portal/cluster-settings-overview.png "HDInsight kümesi özellikleri")

## <a name="run-apache-hive-queries"></a>Apache Hive sorguları çalıştırma

[Apache Hive](hdinsight-use-hive.md) HDInsight’ta kullanılan en popüler bileşendir. HDInsight’ta Hive işleri çalıştırmanın birçok yolu vardır. Bu hızlı başlangıçta portaldan ambarı Hive görünümünü kullanırsınız. Hive işlerini göndermenin diğer yöntemleri için bkz. [HDInsight’ta Hive kullanma](hdinsight-use-hive.md).

> [!NOTE]
> Apache Hive görünümü HDInsight 4,0 ' de kullanılamaz.

1. Ambari’yi açmak için, önceki ekran görüntüsünden **Küme Panosu**’nu seçin.  Ayrıca, önceki bölümde oluşturduğunuz küme `ClusterName` `https://ClusterName.azurehdinsight.net` da gidebilirsiniz.

    ![HDInsight Linux kullanmaya başlama kümesi panosu](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-open-cluster-dashboard.png "HDInsight Linux kullanmaya başlama kümesi panosu")

2. Kümeyi oluştururken belirlediğiniz Hadoop kullanıcı adını ve parolasını girin. Varsayılan kullanıcı adı **admin** şeklindedir.

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Hive Görünümü**’nü açın:

    ![Ambarı 'ndan Hive görünümü seçme](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdi-select-hive-view.png "HDInsight Hive Görüntüleyicisi menüsü")

4. Sayfadaki **SORGU** sekmesinde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:

    ```sql
    SHOW TABLES;
    ```

    ![HDInsight Hive görünümü sorgu Düzenleyicisi](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdi-apache-hive-view1.png "HDInsight Hive görünümü sorgu Düzenleyicisi")

5. **Yürüt**’ü seçin. **SORGU** sekmesinin altında bir **SONUÇLAR** sekmesi görünür. Bu sekmede işle ilgili bilgiler görüntülenir. 

    Sorgu tamamlandıktan sonra **sorgu** sekmesi işlemin sonuçlarını görüntüler. **hivesampletable** adlı bir tablo görürsünüz. Bu örnek Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.

    ![HDInsight Apache Hive sonuçları görüntüle](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-hive-views.png "HDInsight Apache Hive sonuçları görüntüle")

6. Aşağıdaki sorguyu çalıştırmak için 4. ve 5. adımı yineleyin:

    ```sql
    SELECT * FROM hivesampletable;
    ```

7. Ayrıca sorgunun sonuçlarını da kaydedebilirsiniz. Sağdaki menü düğmesini seçtikten sonra, sonuçları CSV dosyası olarak indirme veya kümeyle ilişkili depolama hesabında depolama seçeneklerinden birini belirleyin.

    ![Apache Hive sorgusunun sonucunu Kaydet](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-hive-view-save-results.png "Apache Hive sorgusunun sonucunu Kaydet")

Bir Hive işini tamamladıktan sonra, [sonuçları Azure SQL veritabanı 'na veya SQL Server veritabanına aktarabilirsiniz](apache-hadoop-use-sqoop-mac-linux.md), ayrıca [Excel kullanarak sonuçları görselleştirebilirsiniz](apache-hadoop-connect-excel-power-query.md). HDInsight 'ta Hive kullanma hakkında daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop ile Apache Hive ve HiveQL kullanarak örnek Apache Log4J dosyası çözümleme](hdinsight-use-hive.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile Verileriniz Azure Storage 'da depolanır, bu sayede bir kümeyi kullanımda olmadığında güvenle silebilirsiniz. Ayrıca, kullanımda olmasa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Kümenin ücretleri depolama ücretinden çok daha fazla olduğundan, kullanımda olmadıkları zaman kümeleri silmek ekonomik bir anlam sağlar.

> [!NOTE]  
> HDInsight 'ta Hadoop kullanarak ETL işlemlerini nasıl çalıştıracağınızı öğrenmek için *hemen* bir sonraki makaleye devam ediyorsanız, kümeyi çalışır durumda tutmak isteyebilirsiniz. Bunun nedeni öğreticide bir Hadoop kümesi oluşturmanız gerekir. Ancak, bir sonraki makaleyi hemen ilerleyecekseniz kümeyi şimdi silmeniz gerekir.

### <a name="to-delete-the-cluster-andor-the-default-storage-account"></a>Küme ve/veya varsayılan depolama hesabını silmek için

1. Azure portalın bulunduğu tarayıcı sekmesine dönün. Kümeye genel bakış sayfasında olmalısınız. Yalnızca kümeyi silmek, ancak varsayılan depolama hesabını korumak istiyorsanız **Sil**’i seçin.

    ![Azure HDInsight küme silme](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-delete-cluster.png "Azure HDInsight kümesini silme")

2. Kümeyi ve varsayılan depolama hesabını silmek istiyorsanız, kaynak grubu sayfasını açmak için kaynak grubu adını (önceki ekran görüntüsünde vurgulanan) seçin.

3. **Kaynak grubunu sil**’i seçerek, kümeyi ve varsayılan depolama hesabını içeren kaynak grubunu silin. Kaynak grubu silindiğinde depolama hesabının da silindiğini unutmayın. Depolama hesabını tutmak istiyorsanız, yalnızca küme silmeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kaynak Yöneticisi şablonu kullanarak Linux tabanlı HDInsight kümesi oluşturmayı ve temel Hive sorguları gerçekleştirmeyi öğrendiniz. Sonraki makalede, HDInsight üzerinde Hadoop kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemi gerçekleştirmeyi öğreneceksiniz.

> [!div class="nextstepaction"]
> [HDInsight üzerinde etkileşimli sorgu kullanarak verileri ayıklama, dönüştürme ve yükleme](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
