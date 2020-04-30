---
title: "Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure HDInsight 'ta Apache Hadoop kümesi oluşturma"
description: Bu hızlı başlangıçta, Kaynak Yöneticisi şablonu kullanarak Azure HDInsight 'ta Apache Hadoop kümesi oluşturacaksınız
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/13/2020
ms.openlocfilehash: ae0f29b8085bd9637f527f2a58229dd89ce6933b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80064669"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure HDInsight 'ta Apache Hadoop kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight 'ta bir [Apache Hadoop](./apache-hadoop-introduction.md) kümesi oluşturmak için bir Azure Resource Manager şablonu kullanırsınız. Hadoop, kümelerdeki büyük veri kümelerinin dağıtılmış işleme ve çözümlemesine yönelik orijinal açık kaynaklı çerçevedir. Hadoop ekosistemi Apache Hive, Apache HBase, Spark, Kafka ve diğer birçok tane dahil olmak üzere ilgili yazılım ve yardımcı programları içerir.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]
  
Şu anda HDInsight [yedi farklı küme türüyle](../hdinsight-overview.md#cluster-types-in-hdinsight) ile birlikte gelir. Her küme türü farklı bir bileşen kümesini destekler. Tüm küme türleri Hive'ı destekler. HDInsight’ta desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler nelerdir?](../hdinsight-component-versioning.md)  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-apache-hadoop-cluster"></a>Apache Hadoop kümesi oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password).

:::code language="json" source="~/quickstart-templates/101-hdinsight-linux-ssh-password/azuredeploy.json" range="1-148":::


Şablonda iki Azure kaynağı tanımlanmıştır:

* [Microsoft. Storage/storageAccounts](https://docs.microsoft.com/azure/templates/microsoft.storage/storageaccounts): bir Azure depolama hesabı oluşturun.
* [Microsoft. HDInsight/Cluster](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/clusters): HDInsight kümesi oluşturma.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure 'da oturum açmak ve Kaynak Yöneticisi şablonunu açmak için aşağıdaki **Azure 'A dağıt** düğmesini seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hadoop-linux-tutorial-get-started/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

1. Aşağıdaki değerleri yazın veya seçin:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Abonelik|Aşağı açılan listeden, küme için kullanılan Azure aboneliğini seçin.|
    |Kaynak grubu|Aşağı açılan listeden, mevcut kaynak grubunuzu seçin veya **Yeni oluştur**' u seçin.|
    |Konum|Değer, kaynak grubu için kullanılan konum ile tekrar doldurulur.|
    |Küme Adı|Genel olarak benzersiz bir ad girin. Bu şablon için yalnızca küçük harfler ve rakamlar kullanın.|
    |Küme Türü | **hadoop**’u seçin. |
    |Küme Oturum Açma Kullanıcı Adı|Kullanıcı adını belirtin, varsayılan olarak **admin**' dir.|
    |Küme Oturum Açma Parolası|Bir parola belirtin. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, bir büyük harf ve bir küçük harf, bir alfasayısal olmayan karakter (' "' karakterleri dışında) içermelidir. |
    |SSH Kullanıcı adı|Kullanıcı adını belirtin, varsayılan olarak **sshuser**|
    |SSH parolası|Parolayı girin.|

    Şablondaki bazı özellikler sabit kodlanmış olabilir.  Bu değerleri şablondan yapılandırabilirsiniz. Bu özellikler hakkında daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

    > [!NOTE]  
    > Sağladığınız değerler benzersiz olmalı ve adlandırma yönergelerini izlemelidir. Şablon, doğrulama denetimleri gerçekleştirmez. Sağladığınız değerler zaten kullanılıyorsa veya yönergelere uygun değilse, şablonu gönderdikten sonra bir hata alırsınız.  

    ![HDInsight Linux, portalda Kaynak Yöneticisi şablonu başlattı](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "Azure portal ve bir kaynak grubu yöneticisi şablonunu kullanarak HDInsight 'ta Hadoop kümesi dağıtma")

1. **Hüküm ve koşulları**gözden geçirin. Ardından, **yukarıda belirtilen hüküm ve koşulları kabul ediyorum**' u ve ardından **satın al**' ı seçin. Dağıtımınızın devam ettiğini belirten bir bildirim alırsınız. Bir küme oluşturmak yaklaşık 20 dakika sürer.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Küme oluşturulduktan sonra **Kaynağa Git** bağlantısına sahip bir **dağıtım başarılı** bildirimi alırsınız. Kaynak grubu sayfanız yeni HDInsight kümenizi ve kümeyle ilişkili varsayılan depolamayı listeler. Her kümenin bir [Azure depolama](../hdinsight-hadoop-use-blob-storage.md) hesabı veya [Azure Data Lake Storage hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Bu, varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Küme silme, depolama hesabını silmez.

> [!NOTE]  
> Diğer küme oluşturma yöntemleri ve bu hızlı başlangıçta kullanılan özellikleri anlamak için bkz. [HDInsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile Verileriniz Azure Storage 'da depolanır, bu sayede bir kümeyi kullanımda olmadığında güvenle silebilirsiniz. Ayrıca, kullanımda olmasa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Kümenin ücretleri depolama ücretinden çok daha fazla olduğundan, kullanımda olmadıkları zaman kümeleri silmek ekonomik bir anlam sağlar.

> [!NOTE]  
> HDInsight üzerinde Hadoop kullanarak ETL işlemlerinin nasıl çalıştırılacağını öğrenmek için sonraki öğreticiye *hemen* ilerliyorsanız, kümeyi çalışır durumda tutmak isteyebilirsiniz. Bunun nedeni öğreticide bir Hadoop kümesi oluşturmanız gerekir. Ancak sonraki öğreticiye hemen geçmeyecekseniz şimdi kümeyi silmeniz gerekir.

Azure portal kümenize gidin ve **Sil**' i seçin.

![HDInsight kümeyi portaldan Sil](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "HDInsight kümeyi portaldan Sil")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kaynak Yöneticisi şablonu kullanarak HDInsight 'ta Apache Hadoop kümesi oluşturmayı öğrendiniz. Sonraki makalede, HDInsight üzerinde Hadoop kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemi gerçekleştirmeyi öğreneceksiniz.

> [!div class="nextstepaction"]
> [HDInsight üzerinde etkileşimli sorgu kullanarak verileri ayıklama, dönüştürme ve yükleme](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
