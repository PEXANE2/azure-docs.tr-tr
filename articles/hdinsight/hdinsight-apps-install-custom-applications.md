---
title: Azure HDInsight'ta özel Apache Hadoop uygulamalarınızı yükleyin
description: Azure HDInsight'ta Apache Hadoop kümeleri için HDInsight uygulamalarını nasıl yükleyeceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: b25e9d860f118c1b044b13e01a80aaf7a24963cf
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768240"
---
# <a name="install-custom-apache-hadoop-applications-on-azure-hdinsight"></a>Azure HDInsight'a özel Apache Hadoop uygulamalarını yükleyin

Bu makalede, Azure portalında yayınlanmamış olan Azure HDInsight'a [apache Hadoop](https://hadoop.apache.org/) uygulamasını nasıl yükleyeceğinizi öğreneceksiniz. Bu makalede yükleyeceğiniz uygulama [Hue'dur.](https://gethue.com/)

HDInsight uygulaması, kullanıcıların bir HDInsight kümesine yükleyebileceği bir uygulamadır.  Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir.  

## <a name="prerequisites"></a>Ön koşullar

HDInsight uygulamalarını mevcut bir HDInsight kümesine yüklemek istiyorsanız bir HDInsight kümesine sahip olmanız gerekir. Küme oluşturmak için bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md). HDInsight uygulamalarını ayrıca bir HDInsight kümesi oluştururken yükleyebilirsiniz.

## <a name="install-hdinsight-applications"></a>HDInsight uygulamaları yükleme

HDInsight uygulamaları bir küme oluşturduğunuzda veya var olan bir HDInsight kümesine yüklenebilir. Azure Resource Manager şablonlarını tanımlamak için bkz. [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx).

Bu uygulamayı (Hue) dağıtmak için gerekli dosyalar:

* [azuredeploy.json](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): HDInsight uygulamasını yüklemek için Azure Resource Manager şablonu. Kendi Azure Resource Manager şablonunuzu geliştirmek için bkz. [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx).
* [hue-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): Kenar düğümünü yapılandırmak üzere Resource Manager şablonu tarafından çağrılan Betik eylemi.
* [hue-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): hui-install_v0.sh dosyasından çağrılan Hue ikili dosyası.
* [hue-binaries-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): hui-install_v0.sh dosyasından çağrılan Hue ikili dosyası.
* [webwasb-tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): hui-install_v0.sh dosyasından çağrılan bir örnek web uygulaması (Tomcat).

### <a name="to-install-hue-to-an-existing-hdinsight-cluster"></a>Var olan bir HDInsight kümesine Hue yüklemek için

1. Azure'da oturum açmak ve Azure portalında Kaynak Yöneticisi şablonu açmak için aşağıdaki resmi seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

    Kaynak Yöneticisi şablonu [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).  Bu Resource Manager şablonunun nasıl yazılacağını öğrenmek için bkz. [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx).

1. Açılan listeden kümenizi içeren varolan **Kaynak grubunu** seçin. Kümeyle aynı kaynak grubunu kullanmak gerekir.

1. Uygulamayı yüklemek istediğiniz kümenin adını girin. Bu küme var olan bir küme olmalıdır.

1. **Yukarıda belirtilen hüküm ve koşulları kabul ettiğim için**onay kutusunu seçin.

1. **Satın al**'ı seçin.

Yükleme durumunu portal panosuna sabitlenmiş kutucuktan ve portal bildiriminden görebilirsiniz (portalın üst kısmındaki zil simgesine tıklayın).  Uygulamanın yüklenmesi yaklaşık 10 dakika sürer.

### <a name="to-install-hue-while-creating-a-cluster"></a>Küme oluştururken Hue yüklemek için

1. Azure'da oturum açmak ve Azure portalında Kaynak Yöneticisi şablonu açmak için aşağıdaki resmi seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

    Kaynak Yöneticisi şablonu [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).  Bu Resource Manager şablonunun nasıl yazılacağını öğrenmek için bkz. [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx).

2. Küme oluşturmak ve Hue uygulamasını yüklemek için yönergeleri izleyin. HDInsight kümeleri oluşturma hakkında daha fazla bilgi için bkz. [HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="other-installation-methods"></a>Diğer yükleme yöntemleri

Resource Manager şablonlarını çağırmak için Azure portalına ek olarak [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-using-powershell) ve [Azure CLI](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-using-azure-cli) kullanabilirsiniz.

## <a name="validate-the-installation"></a>Yüklemeyi doğrulama

Uygulama yüklemesini doğrulamak için Azure portalında uygulama durumunu denetleyebilirsiniz. Buna ek olarak, tüm HTTP uç noktaları nın beklendiği gibi ortaya çıktığını ve varsa web sayfasının da geçerli olduğunu doğrulayabilirsiniz.

**Hue**için aşağıdaki adımları kullanabilirsiniz:

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Uygulamayı yüklediğiniz kümeyi seçin.
1. **Ayarlar** menüsünden **Uygulamalar'ı**seçin.
1. Özellikleri görüntülemek için listeden **ton** seçin.  
1. Web sitesini doğrulamak için Web sayfası bağlantısını seçin.

### <a name="azure-cli"></a>Azure CLI

Değiştirin `CLUSTERNAME` `RESOURCEGROUP` ve ilgili değerlerle ve ardından aşağıdaki komutları girin:

* HDInsight kümesi için tüm uygulamaları listeler.

    ```azurecli
    az hdinsight application list --cluster-name CLUSTERNAME --resource-group RESOURCEGROUP
    ```

* Belirtilen uygulamanın özelliklerini almak için.

    ```azurecli
    az hdinsight application show --name hue --cluster-name CLUSTERNAME --resource-group RESOURCEGROUP
    ```

## <a name="troubleshoot-the-installation"></a>Yükleme sorunlarını giderme

Uygulama yükleme durumunu portal bildiriminden denetleyebilirsiniz (Portalın üst kısmındaki zil simgesine tıklayın).

Bir uygulama yüklemesi başarısız olduysa, hata iletilerini görebilir ve bilgileri üç yerden ayıklayabilirsiniz:

* HDInsight Uygulamaları: genel hata bilgileri.

    Kümeyi portaldan açın ve Ayarlar'dan Uygulamalar'ı seçin:

    ![hdinsight applications application installation error](./media/hdinsight-apps-install-custom-applications/hdinsight-apps-error.png)

* HDInsight betik eylemi: HDInsight Uygulamalarının hata iletisi bir betik eylemi hatası belirtiyorsa betik eylemleri bölmesinde betik hatasına ilişkin daha fazla bilgi sunulur.

    Ayarlar'dan Komut Dosyası Eylemi'ni seçin. Betik eylemi geçmişinde hata iletileri gösterilir

    ![hdinsight applications script action error](./media/hdinsight-apps-install-custom-applications/hdinsight-apps-script-action-error.png)

* Apache Ambari Web UI: Yükleme komut dosyası hatanın nedeniyse, yükleme komut dosyaları yla ilgili tam günlükleri denetlemek için Ambari Web UI'yi kullanın.

    Daha fazla bilgi için sorun [giderme komut dosyası eylemleri'ne](./troubleshoot-script-action.md)bakın.

## <a name="remove-hdinsight-applications"></a>HDInsight uygulamalarını kaldırma

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Uygulamayı yüklediğiniz kümeyi seçin.
1. **Ayarlar** menüsünden **Uygulamalar'ı**seçin.
1. Kaldırmak istediğiniz uygulamayı sağ tıklatın ve sonra **Sil'i**seçin.
1. Onaylamak için **Evet**'i seçin.

### <a name="azure-cli"></a>Azure CLI

Değiştir `NAME` `CLUSTERNAME`, `RESOURCEGROUP` ve ilgili değerlerle ve ardından aşağıdaki komutu girin:

```azurecli
az hdinsight application delete --name NAME --cluster-name CLUSTERNAME --resource-group RESOURCEGROUP
```

## <a name="next-steps"></a>Sonraki adımlar

* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını dağıtmaya yönelik Resource Manager şablonlarını nasıl geliştireceğinizi öğrenin.
* [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md): HDInsight uygulamalarını kümelerinize yükleme hakkında bilgi alın.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Linux tabanlı Apache Hadoop kümeleri oluşturun:](hdinsight-hadoop-create-linux-clusters-arm-templates.md)HDInsight kümeleri oluşturmak için Kaynak Yöneticisi şablonlarını nasıl çağıracağınız öğrenin.
* [HDInsight’ta boş kenar düğümleri kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümesine erişmek, HDInsight uygulamaları test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.
