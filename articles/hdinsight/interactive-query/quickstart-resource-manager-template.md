---
title: 'Quickstart: Şablonu kullanarak Etkileşimli Sorgu kümesi oluşturma - Azure HDInsight'
description: Bu hızlı başlangıç, Azure HDInsight'ta Etkileşimli Sorgu kümesi oluşturmak için Kaynak Yöneticisi şablonunun nasıl kullanılacağını gösterir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/13/2020
ms.openlocfilehash: 02dbb129fa32ce72a94a098c56b0a81186783ffd
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79485821"
---
# <a name="quickstart-create-interactive-query-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure HDInsight'ta Etkileşimli Sorgu kümesi oluşturma

Bu hızlı başlatmada, Azure HDInsight'ta [Etkileşimli Sorgu](./apache-interactive-query-get-started.md) kümesi oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanırsınız. Etkileşimli Sorgu (Apache Hive LLAP veya [Düşük GecikmeLi Analitik İşleme](https://cwiki.apache.org/confluence/display/Hive/LLAP)olarak da adlandırılır) Bir Azure HDInsight [küme türüdür.](../hdinsight-hadoop-provision-linux-clusters.md#cluster-type)

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-interactive-query-cluster"></a>Etkileşimli Sorgu kümesi Oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-interactive-hive)

:::code language="json" source="~/quickstart-templates/101-hdinsight-interactive-hive/azuredeploy.json" range="1-158":::


Şablonda iki Azure kaynağı tanımlanır:

* [Microsoft.Storage/storageHesapları](https://docs.microsoft.com/azure/templates/microsoft.storage/storageaccounts): bir Azure Depolama Hesabı oluşturun.
* [Microsoft.HDInsight/cluster](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/clusters): bir HDInsight kümesi oluşturun.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure'da oturum açmak ve Kaynak Yöneticisi şablonunu açmak için aşağıdaki **Azure'a Dağıt** düğmesini seçin.

    [![Azure'a Dağıt](./media/quickstart-resource-manager-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-interactive-hive%2Fazuredeploy.json)

1. Aşağıdaki değerleri yazın veya seçin:

    |Özellik |Açıklama |
    |---|---|
    |Abonelik|Açılan listeden, küme için kullanılan Azure aboneliğini seçin.|
    |Kaynak grubu|Açılan listeden, varolan kaynak grubunuzu seçin veya **yeni oluştur'u**seçin.|
    |Konum|Değer, kaynak grubu için kullanılan konumla otomatik olarak doldurulur.|
    |Küme Adı|Genel olarak benzersiz bir ad girin. Bu şablon için yalnızca küçük harfleri ve sayıları kullanın.|
    |Küme Oturum Açma Kullanıcı Adı|Kullanıcı adı sağlayın, varsayılan **yönetici**.|
    |Küme Oturum Açma Parolası|Bir parola sağlayın. Parola en az 10 karakter uzunluğunda olmalı ve en az bir basamak, bir büyük harf ve bir küçük harf, bir alfasayısal olmayan karakter içermelidir (karakterler hariç ' ' ' ). |
    |Ssh Kullanıcı Adı|Kullanıcı adı sağlayın, varsayılan sshuser olduğunu|
    |Ssh Şifre|Parolayı girin.|

    ![Kaynak Yöneticisi şablonu HBase](./media/quickstart-resource-manager-template/resource-manager-template-hive.png)

1. **ŞARTLARI VE KOŞULLARI**Gözden Geçirin. Sonra **ben yukarıda belirtilen hüküm ve koşulları kabul**seçin , sonra Satın **Al**. Dağıtımınızın devam ettiğine dair bir bildirim alırsınız. Bir küme oluşturmak yaklaşık 20 dakika sürer.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Küme oluşturulduktan sonra, **kaynak bağlantısına Git** ile **dağıtım başarılı** bildirimi alırsınız. Kaynak grubu sayfanız yeni HDInsight kümenizi ve kümeyle ilişkili varsayılan depolama alanını listeler. Her kümenin bir [Azure Depolama](../hdinsight-hadoop-use-blob-storage.md) hesabı veya Azure Veri Gölü [Depolama hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Kümeleri silmek depolama hesabını silmez.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

Azure portalından kümenize gidin ve **Sil'i**seçin.

![Kaynak Yöneticisi şablonu HBase](./media/quickstart-resource-manager-template/azure-portal-delete-hive.png)

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Bir Kaynak Yöneticisi şablonu kullanarak HDInsight'ta Etkileşimli Sorgu kümesi oluşturmayı öğrendiniz. Sonraki makalede, Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanmayı öğrenirler.

> [!div class="nextstepaction"]
> [Apache Zeppelin ile Azure HDInsight'ta Apache Hive sorgularını yürütün](./hdinsight-connect-hive-zeppelin.md)
