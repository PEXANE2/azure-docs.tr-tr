---
title: 'Quickstart: Şablonu kullanarak ML Hizmetleri kümesi oluşturma - Azure HDInsight'
description: Bu hızlı başlangıç, Azure HDInsight'ta bir ML Hizmetleri kümesi oluşturmak için Kaynak Yöneticisi şablonunun nasıl kullanılacağını gösterir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/13/2020
ms.openlocfilehash: cde8d6932400966ae22720b1e86f3c5164f25b30
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81603439"
---
# <a name="quickstart-create-ml-services-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure HDInsight'ta ML Hizmetleri kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight'ta bir [ML Hizmetleri](./r-server-overview.md) kümesi oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanırsınız. Microsoft Machine Learning Server, Azure'da HDInsight kümeleri oluşturduğunuzda dağıtım seçeneği olarak kullanılabilir. Bu seçeneği sağlayan küme türüNE ML Hizmetleri denir. Bu özellik, HDInsight'ta ölçeklenebilir, dağıtılmış analiz yöntemlerine isteğe bağlı erişim sağlayan veri bilimcileri, istatistikçiler ve R programcıları sağlar.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-ml-services-cluster"></a>ML Hizmetleri kümesi oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-rserver)

:::code language="json" source="~/quickstart-templates/101-hdinsight-rserver/azuredeploy.json" range="1-171":::


Şablonda iki Azure kaynağı tanımlanır:

* [Microsoft.Storage/storageHesapları](https://docs.microsoft.com/azure/templates/microsoft.storage/storageaccounts): bir Azure Depolama Hesabı oluşturun.
* [Microsoft.HDInsight/cluster](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/clusters): bir HDInsight kümesi oluşturun.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure'da oturum açmak ve Kaynak Yöneticisi şablonunu açmak için aşağıdaki **Azure'a Dağıt** düğmesini seçin.

    [![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-rserver%2Fazuredeploy.json)

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

    ![Kaynak Yöneticisi şablonu HBase](./media/quickstart-resource-manager-template/resource-manager-template-rserver.png)

1. **ŞARTLARI VE KOŞULLARI**Gözden Geçirin. Sonra **ben yukarıda belirtilen hüküm ve koşulları kabul**seçin , sonra Satın **Al**. Dağıtımınızın devam ettiğine dair bir bildirim alırsınız. Bir küme oluşturmak yaklaşık 20 dakika sürer.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Küme oluşturulduktan sonra, **kaynak bağlantısına Git** ile **dağıtım başarılı** bildirimi alırsınız. Kaynak grubu sayfanız yeni HDInsight kümenizi ve kümeyle ilişkili varsayılan depolama alanını listeler. Her kümenin bir [Azure Depolama](../hdinsight-hadoop-use-blob-storage.md) hesabı veya Azure Veri Gölü [Depolama hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Varsayılan depolama hesabı olarak adlandırılır. HDInsight kümesi ve varsayılan depolama hesabı aynı Azure bölgesinde birlikte bulunmalıdır. Kümeleri silmek depolama hesabını silmez.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

Azure portalından kümenize gidin ve **Sil'i**seçin.

![Kaynak Yöneticisi şablonu HBase](./media/quickstart-resource-manager-template/azure-portal-delete-rserver.png)

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Kaynak Yöneticisi şablonu kullanarak HDInsight'ta ML Hizmetleri kümesioluşturmayı öğrendiniz. Sonraki makalede, dağıtılmış R hesaplamaları için Spark'ı kullandığını gösteren RStudio Server ile nasıl çalıştırılabilirsiniz...

> [!div class="nextstepaction"]
> [RStudio Server'ı kullanarak Azure HDInsight'ta ML Hizmetleri kümesinde Bir R komut dosyası çalıştırma](./machine-learning-services-quickstart-job-rstudio.md)
