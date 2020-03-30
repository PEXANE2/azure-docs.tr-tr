---
title: 'Hızlı başlangıç: Apache Hadoop, Apache Hive & Azure HDInsight portalı'
description: Bu hızlı başlangıçta, bir HDInsight Hadoop kümesi oluşturmak için Azure portalını kullanırsınız
keywords: hadoop kullanmaya başlama,hadoop linux,hadoop hızlı başlangıç,hive kullanmaya başlama,hive hızlı başlangıç
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.date: 02/24/2020
ms.openlocfilehash: 8939d9b342094342d576c00cf02e622286c8fc0f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80130547"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak Azure HDInsight'ta Apache Hadoop kümesi oluşturma

Bu makalede, Azure portalını kullanarak HDInsight'ta Apache Hadoop kümeleri oluşturmayı ve ardından HDInsight'ta Apache Hive işlerini çalıştırmayı öğreneceksiniz. Hadoop işlerinin çoğu toplu işlemdir. Bir küme oluşturur, bazı işleri çalıştırır ve kümeyi silersiniz. Bu makalede, üç görevi de gerçekleştirirsiniz. Kullanılabilir yapılandırmaların ayrıntılı açıklamaları için [hdinsight'ta kümeler ayarlama](../hdinsight-hadoop-provision-linux-clusters.md)bölümüne bakın. Kümeoluşturmak için portalın kullanımı yla ilgili daha fazla bilgi için [bkz.](../hdinsight-hadoop-create-linux-clusters-portal.md)

Bu hızlı başlangıçta, HDInsight Hadoop kümesi oluşturmak için Azure portalını kullanırsınız. [Azure Resource Manager şablonunu](apache-hadoop-linux-tutorial-get-started.md) kullanarak da küme oluşturabilirsiniz.

Şu anda, HDInsight [yedi farklı küme türleri](../hdinsight-overview.md#cluster-types-in-hdinsight)ile birlikte geliyor. Her küme türü farklı bir bileşen kümesini destekler. Tüm küme türleri Hive'ı destekler. HDInsight'ta desteklenen bileşenlerin listesi için, [HDInsight tarafından sağlanan Apache Hadoop küme sürümlerinde yeniliklere bakın?](../hdinsight-component-versioning.md)  

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="create-an-apache-hadoop-cluster"></a>Apache Hadoop kümesi oluşturma

Bu bölümde, Azure portalını kullanarak HDInsight’ta Hadoop kümesi oluşturursunuz.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Üst menüden **+ Kaynak oluştur'u**seçin.

    ![Kaynak HDInsight kümesi oluşturma](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-create-resource.png "Kaynak HDInsight kümesi oluşturma")

1. **HDInsight oluştur küme** sayfasına gitmek için **Analytics** > **Azure HDInsight'ı** seçin.

1. Temel **Bilgiler** sekmesinden aşağıdaki bilgileri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Abonelik    |  Açılan listeden, küme için kullanılan Azure aboneliğini seçin. |
    |Kaynak grubu     | Açılan listeden, varolan kaynak grubunuzu seçin veya **yeni oluştur'u**seçin.|
    |Küme adı   | Genel olarak benzersiz bir ad girin. Ad, harfler, sayılar ve tireler de dahil olmak üzere en fazla 59 karakterden oluşabilir. Adın ilk ve son karakterleri tire olamaz. |
    |Bölge    | Açılan listeden, kümenin oluşturulduğu bir bölge seçin.  Daha iyi performans için kendinize yakın bir konum seçin. |
    |Küme türü| **Küme türünü seç'i**seçin. Ardından küme türü olarak **Hadoop'u** seçin.|
    |Sürüm|Açılan listeden bir **sürüm**seçin. Ne seçeceğinizi bilmiyorsanız varsayılan sürümü kullanın.|
    |Küme oturum açma kullanıcı adı ve parolası    | Varsayılan giriş adı **yöneticidir.** Parola en az 10 karakter uzunluğunda olmalı ve en az bir basamak, bir büyük harf ve bir küçük harf, bir alfanümerik olmayan karakter içermelidir (karakterler hariç ' ' . \) "Pass@word1" gibi genel parolalar **sağlamadığınızdan** emin olun.|
    |Secure Shell (SSH) kullanıcı adı | Varsayılan kullanıcı adı **sshuser** şeklindedir.  SSH kullanıcı adı için başka bir ad sağlayabilirsiniz. |
    |SSH için küme giriş parolası kullanma| Küme giriş kullanıcısı için sağladığınız parolayla aynı Parolayı Kullanmak için bu onay kutusunu seçin.|

    ![HDInsight Linux küme temel değerleri sağlamak başlamak olsun](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-basics.png "HDInsight kümesi oluşturmak için temel değerleri sağlama")

    **Sonraki: Depolama >>'ni** seçin ve depolama ayarlarına ilerleyin.

1. **Depolama** sekmesinden aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü|Varsayılan değeri kullanın **Azure Depolama**.|
    |Seçim yöntemi|Varsayılan değeri kullanın **Listeden seçin.**|
    |Birincil depolama hesabı|Varolan bir depolama hesabını seçmek için açılır listeyi kullanın veya **yeni hesap oluştur'u**seçin. Yeni bir hesap oluşturursanız, ad uzunluğu 3 ile 24 karakter arasında olmalıdır ve yalnızca sayılar ve küçük harfler içerebilir|
    |Kapsayıcı|Otomatik doldurulan değeri kullanın.|

    ![HDInsight Linux küme depolama değerleri sağlamak başlamak olsun](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-storage.png "HDInsight kümesi oluşturmak için depolama değerleri sağlama")

    Her kümenin bir [Azure Depolama hesabı](../hdinsight-hadoop-use-blob-storage.md) veya [Azure Data Lake hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Kümeleri silmek depolama hesabını silmez.

    Gözden **Geçir + oluştur** sekmesini seçin.

1. Gözden **Geçir + oluştur** sekmesinden, önceki adımlarda seçtiğiniz değerleri doğrulayın.

    ![HDInsight Linux küme özeti ne başlar](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-review-create-hadoop.png "HDInsight Linux küme özeti ne başlar")

1. **Oluştur'u**seçin. Bir küme oluşturmak yaklaşık 20 dakika sürer.

    Küme oluşturulduktan sonra, Azure portalında kümeye genel bakış sayfasını görürsünüz.

    ![HDInsight Linux kullanmaya başlama küme ayarları](./media/apache-hadoop-linux-create-cluster-get-started-portal/cluster-settings-overview.png "HDInsight küme özellikleri")

## <a name="run-apache-hive-queries"></a>Apache Hive sorgularını çalıştırın

[Apache Hive](hdinsight-use-hive.md) HDInsight’ta kullanılan en popüler bileşendir. HDInsight’ta Hive işleri çalıştırmanın birçok yolu vardır. Bu hızlı başlangıçta, portaldan Ambari Hive görünümünü kullanırsınız. Hive işlerini göndermenin diğer yöntemleri için bkz. [HDInsight’ta Hive kullanma](hdinsight-use-hive.md).

> [!NOTE]
> Apache Hive View HDInsight 4.0'da kullanılamaz.

1. Ambari’yi açmak için, önceki ekran görüntüsünden **Küme Panosu**’nu seçin.  Ayrıca, önceki bölümde `https://ClusterName.azurehdinsight.net` `ClusterName` oluşturduğunuz kümenin nerede olduğuna da göz atabilirsiniz.

    ![HDInsight Linux küme panosuna başlıyor](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-open-cluster-dashboard.png "HDInsight Linux küme panosuna başlıyor")

2. Kümeyi oluştururken belirlediğiniz Hadoop kullanıcı adını ve parolasını girin. Varsayılan kullanıcı adı **admin** şeklindedir.

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Hive Görünümü**’nü açın:

    ![Ambari'den Kovan Görünümü Seçme](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdi-select-hive-view.png "HDInsight Hive Görüntüleyici menüsü")

4. Sayfadaki **SORGU** sekmesinde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:

    ```sql
    SHOW TABLES;
    ```

    ![HDInsight Kovan Görünümü Sorgu Düzenleyicisi](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdi-apache-hive-view1.png "HDInsight Kovan Görünümü Sorgu Düzenleyicisi")

5. **Yürüt**’ü seçin. **SORGU** sekmesinin altında bir **SONUÇLAR** sekmesi görünür. Bu sekmede işle ilgili bilgiler görüntülenir. 

    Sorgu tamamlandıktan sonra, **SORGU** sekmesi işlemin sonuçlarını görüntüler. **hivesampletable** adlı bir tablo görürsünüz. Bu örnek Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.

    ![HDInsight Apache Hive görüntüleme sonuçları](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-hive-views.png "HDInsight Apache Hive görüntüleme sonuçları")

6. Aşağıdaki sorguyu çalıştırmak için 4. ve 5. adımı yineleyin:

    ```sql
    SELECT * FROM hivesampletable;
    ```

7. Ayrıca sorgunun sonuçlarını da kaydedebilirsiniz. Sağdaki menü düğmesini seçtikten sonra, sonuçları CSV dosyası olarak indirme veya kümeyle ilişkili depolama hesabında depolama seçeneklerinden birini belirleyin.

    ![Apache Hive sorgusunun sonucunu kaydetme](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-hive-view-save-results.png "Apache Hive sorgusunun sonucunu kaydetme")

Bir Hive işini tamamladıktan sonra, [sonuçları Azure SQL Veritabanı'na veya SQL Server veritabanına dışa](apache-hadoop-use-sqoop-mac-linux.md)aktarabilirsiniz, [excel'i kullanarak da sonuçları görselleştirebilirsiniz.](apache-hadoop-connect-excel-power-query.md) HDInsight'ta Hive kullanma hakkında daha fazla bilgi için, [örnek apache log4j dosyasını analiz etmek için HDInsight'ta Apache Hadoop ile Apache Hive ve HiveQL kullanın'](hdinsight-use-hive.md)a bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

> [!NOTE]  
> HDInsight'ta Hadoop kullanarak ETL işlemlerini nasıl çalıştırabileceğinizi öğrenmek için *hemen* bir sonraki makaleye devam ediyorsanız, kümenin çalışmasını sağlamak isteyebilirsiniz. Bunun nedeni, öğreticide tekrar bir Hadoop kümesi oluşturmanız olmasıdır. Ancak, bir sonraki makaleyi hemen gözden geçirmiyorsanız, kümeyi şimdi silmeniz gerekir.

### <a name="to-delete-the-cluster-andor-the-default-storage-account"></a>Küme ve/veya varsayılan depolama hesabını silmek için

1. Azure portalın bulunduğu tarayıcı sekmesine dönün. Kümeye genel bakış sayfasında olmalısınız. Yalnızca kümeyi silmek, ancak varsayılan depolama hesabını korumak istiyorsanız **Sil**’i seçin.

    ![Azure HDInsight silme kümesi](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-delete-cluster.png "Azure HDInsight kümesini silme")

2. Kümeyi ve varsayılan depolama hesabını silmek istiyorsanız, kaynak grubu sayfasını açmak için kaynak grubu adını (önceki ekran görüntüsünde vurgulanan) seçin.

3. **Kaynak grubunu sil**’i seçerek, kümeyi ve varsayılan depolama hesabını içeren kaynak grubunu silin. Kaynak grubu silindiğinde depolama hesabının da silindiğini unutmayın. Depolama hesabını tutmak istiyorsanız, yalnızca küme silmeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Kaynak Yöneticisi şablonu kullanarak Linux tabanlı bir HDInsight kümesi oluşturmayı ve temel Hive sorgularını nasıl gerçekleştireceklerini öğrendiniz. Sonraki makalede, HDInsight üzerinde Hadoop kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemi gerçekleştirmeyi öğreneceksiniz.

> [!div class="nextstepaction"]
> [HDInsight'ta Etkileşimli Sorgu'u kullanarak verileri ayıklayın, dönüştürün ve yükleyin](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
