---
title: "Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure HDInsight'ta Apache Hadoop kümesi oluşturma"
description: Bu hızlı başlangıçta, Kaynak Yöneticisi şablonu kullanarak Azure HDInsight'ta Apache Hadoop kümesini oluşturursunuz
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/13/2020
ms.openlocfilehash: ae0f29b8085bd9637f527f2a58229dd89ce6933b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80064669"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure HDInsight'ta Apache Hadoop kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight'ta [Bir Apache Hadoop](./apache-hadoop-introduction.md) kümesi oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanırsınız. Hadoop, kümeler üzerindeki büyük veri kümelerinin dağıtılmış işleme ve analizi için orijinal açık kaynak çerçevesiydi. Hadoop ekosistemi, Apache Hive, Apache HBase, Spark, Kafka ve diğerleri dahil olmak üzere ilgili yazılım ve yardımcı programları içerir.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]
  
Şu anda HDInsight [yedi farklı küme türüyle](../hdinsight-overview.md#cluster-types-in-hdinsight) ile birlikte gelir. Her küme türü farklı bir bileşen kümesini destekler. Tüm küme türleri Hive'ı destekler. HDInsight’ta desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler nelerdir?](../hdinsight-component-versioning.md)  

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-apache-hadoop-cluster"></a>Apache Hadoop kümesi oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password)

:::code language="json" source="~/quickstart-templates/101-hdinsight-linux-ssh-password/azuredeploy.json" range="1-148":::


Şablonda iki Azure kaynağı tanımlanır:

* [Microsoft.Storage/storageHesapları](https://docs.microsoft.com/azure/templates/microsoft.storage/storageaccounts): bir Azure Depolama Hesabı oluşturun.
* [Microsoft.HDInsight/cluster](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/clusters): bir HDInsight kümesi oluşturun.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure'da oturum açmak ve Kaynak Yöneticisi şablonunu açmak için aşağıdaki **Azure'a Dağıt** düğmesini seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hadoop-linux-tutorial-get-started/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

1. Aşağıdaki değerleri yazın veya seçin:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Abonelik|Açılan listeden, küme için kullanılan Azure aboneliğini seçin.|
    |Kaynak grubu|Açılan listeden, varolan kaynak grubunuzu seçin veya **yeni oluştur'u**seçin.|
    |Konum|Değer, kaynak grubu için kullanılan konumla otomatik olarak doldurulur.|
    |Küme Adı|Genel olarak benzersiz bir ad girin. Bu şablon için yalnızca küçük harfleri ve sayıları kullanın.|
    |Küme Türü | **hadoop**’u seçin. |
    |Küme Oturum Açma Kullanıcı Adı|Kullanıcı adı sağlayın, varsayılan **yönetici**.|
    |Küme Oturum Açma Parolası|Bir parola sağlayın. Parola en az 10 karakter uzunluğunda olmalı ve en az bir basamak, bir büyük harf ve bir küçük harf, bir alfasayısal olmayan karakter içermelidir (karakterler hariç ' ' ' ). |
    |Ssh Kullanıcı Adı|Kullanıcı adı sağlayın, varsayılan **sshuser** olduğunu|
    |Ssh Şifre|Parolayı girin.|

    Şablondaki bazı özellikler sabit kodlanmış olabilir.  Bu değerleri şablondan yapılandırabilirsiniz. Bu özelliklerin daha fazla açıklaması için, [HDInsight'ta Apache Hadoop kümeleri oluştur'a](../hdinsight-hadoop-provision-linux-clusters.md)bakın.

    > [!NOTE]  
    > Sağladığınız değerler benzersiz olmalı ve adlandırma yönergelerini izlemelidir. Şablon, doğrulama denetimleri gerçekleştirmez. Sağladığınız değerler zaten kullanılıyorsa veya yönergelere uygun değilse, şablonu gönderdikten sonra bir hata alırsınız.  

    ![HDInsight Linux portalda Kaynak Yöneticisi şablonu başlar](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "Azure portalını ve kaynak grup yöneticisi şablonu kullanarak HDInsight'ta Hadoop kümesini dağıtma")

1. **ŞARTLARI VE KOŞULLARI**Gözden Geçirin. Sonra **ben yukarıda belirtilen hüküm ve koşulları kabul**seçin , sonra Satın **Al**. Dağıtımınızın devam ettiğine dair bir bildirim alırsınız. Bir küme oluşturmak yaklaşık 20 dakika sürer.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Küme oluşturulduktan sonra, **kaynak bağlantısına Git** ile **dağıtım başarılı** bildirimi alırsınız. Kaynak grubu sayfanız yeni HDInsight kümenizi ve kümeyle ilişkili varsayılan depolama alanını listeler. Her kümenin bir [Azure Depolama](../hdinsight-hadoop-use-blob-storage.md) hesabı veya Azure Veri Gölü [Depolama hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Kümeleri silmek depolama hesabını silmez.

> [!NOTE]  
> Diğer küme oluşturma yöntemleri ve bu hızlı başlatmada kullanılan özellikleri anlamak için [bkz.](../hdinsight-hadoop-provision-linux-clusters.md)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

> [!NOTE]  
> HDInsight üzerinde Hadoop kullanarak ETL işlemlerinin nasıl çalıştırılacağını öğrenmek için sonraki öğreticiye *hemen* ilerliyorsanız, kümeyi çalışır durumda tutmak isteyebilirsiniz. Bunun nedeni, öğreticide tekrar bir Hadoop kümesi oluşturmanız olmasıdır. Ancak sonraki öğreticiye hemen geçmeyecekseniz şimdi kümeyi silmeniz gerekir.

Azure portalından kümenize gidin ve **Sil'i**seçin.

![HDInsight sil küme portalından](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "HDInsight sil küme portalından")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Bir Kaynak Yöneticisi şablonu kullanarak HDInsight'ta bir Apache Hadoop kümesi oluşturmayı öğrendiniz. Sonraki makalede, HDInsight üzerinde Hadoop kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemi gerçekleştirmeyi öğreneceksiniz.

> [!div class="nextstepaction"]
> [HDInsight'ta Etkileşimli Sorgu'u kullanarak verileri ayıklayın, dönüştürün ve yükleyin](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
