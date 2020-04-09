---
title: Şablonları kullanarak Apache Hadoop kümeleri oluşturun - Azure HDInsight
description: Kaynak Yöneticisi şablonlarını kullanarak HDInsight için kümeoluşturmayı öğrenin
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/07/2020
ms.openlocfilehash: 67d5481b82b7469c5ae55704768ce494dbc9dca5
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879148"
---
# <a name="create-apache-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Apache Hadoop kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu makalede, [Azure Kaynak Yöneticisi şablonlarını](../azure-resource-manager/templates/deploy-powershell.md)kullanarak Azure HDInsight kümeleri oluşturmanın çeşitli yollarını öğreniyorsunuz. Diğer küme oluşturma araçları ve özellikleri hakkında bilgi edinmek için bu sayfanın üst kısmındaki sekme seçiciyi tıklatın. Ayrıca bakınız, [Küme oluşturma yöntemleri.](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods)

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Kaynak Yöneticisi şablonu, uygulamanız için aşağıdaki kaynakları tek ve eşgüdümlü bir işlemle oluşturmayı kolaylaştırır:

* HDInsight kümeleri ve bunların bağımlı kaynakları (varsayılan depolama hesabı gibi).
* Diğer kaynaklar [(Apache Sqoop](https://sqoop.apache.org/)kullanmak için Azure SQL Veritabanı gibi).

Şablonda, uygulama için gereken kaynakları tanımlarsınız. Ayrıca, farklı ortamlar için giriş değerleri için dağıtım parametrelerini de belirtirsiniz. Şablon JSON ve dağıtım Için değerler oluşturmak için kullandığınız ifadeler oluşur.

[AZURE quickstart şablonlarında](https://azure.microsoft.com/resources/templates/?term=hdinsight)HDInsight şablon örneklerini bulabilirsiniz. [Şablonu](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) iş istasyonunuzdaki bir dosyaya kaydetmek için Kaynak Yöneticisi uzantısı veya metin düzenleyicisi ile platformlar arası [Visual Studio Kodu'nu](https://code.visualstudio.com/#alt-downloads) kullanın.

Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için aşağıdaki makalelere ve örneklere bakın:

* [Azure Kaynak Yöneticisi şablonlarını yazar](../azure-resource-manager/templates/template-syntax.md)
* [Azure Kaynak Yöneticisi şablonları ile bir uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md)
* [Microsoft.HDInsight/kümeler](/azure/templates/microsoft.hdinsight/allversions) şablon başvurusu
* [Azure hızlı başlatma şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Hdinsight&pageNumber=1&sort=Popular)

## <a name="generate-templates"></a>Şablonoluşturma

Kaynak Yöneticisi, farklı araçları kullanarak aboneliğinizdeki varolan kaynaklardan bir Kaynak Yöneticisi şablonu dışa aktarmanızı sağlar. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz. Daha fazla bilgi için dışa aktarma [şablonlarına](../azure-resource-manager/templates/export-template-portal.md)bakın.

## <a name="deploy-using-the-portal"></a>Portalı kullanarak dağıtma

Azure portalını kullanarak bir Kaynak Yöneticisi şablonu dağıtabilirsiniz. Daha fazla bilgi için [bkz.](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)

## <a name="deploy-using-powershell"></a>PowerShell kullanarak dağıtma

Azure PowerShell'i kullanarak bir Kaynak Yöneticisi şablonu dağıtabilirsiniz. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure PowerShell ile kaynakları dağıt'a](../azure-resource-manager/templates/deploy-powershell.md) bakın ve [SAS jetonu ve Azure PowerShell ile özel Kaynak Yöneticisi şablonu dağıtın.](../azure-resource-manager/resource-manager-powershell-sas-token.md)

## <a name="deploy-using-azure-cli"></a>Azure CLI’yi kullanarak dağıtma

Azure CLI'yi kullanarak bir Kaynak Yöneticisi şablonu dağıtabilirsiniz. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure CLI ile kaynakları dağıt](../azure-resource-manager/templates/deploy-cli.md) ve [SAS jetonu ve Azure CLI ile özel Kaynak Yöneticisi şablonu dağıt'a](../azure-resource-manager/resource-manager-cli-sas-token.md)bakın.

## <a name="deploy-using-the-rest-api"></a>REST API'sini kullanarak dağıtma

REST API'yi kullanarak bir Kaynak Yöneticisi şablonu dağıtabilirsiniz. Daha fazla bilgi için kaynak [yöneticisi şablonları ve Kaynak Yöneticisi REST API ile kaynakları dağıt'a](../azure-resource-manager/templates/deploy-rest.md)bakın.

## <a name="deploy-with-visual-studio"></a>Visual Studio ile dağıtma

 Bir kaynak grubu projesi oluşturmak ve kullanıcı arabirimi aracılığıyla Azure'a dağıtmak için Visual Studio'yu kullanın. Projenize dahil edilecek kaynak türünü seçersiniz. Bu kaynaklar kaynak yöneticisi şablonuna otomatik olarak eklenir. Proje ayrıca şablonu dağıtmak için bir PowerShell komut dosyası sağlar.

Visual Studio'nun kaynak gruplarıyla kullanılmasına giriş yapmak için [Bkz. Visual Studio aracılığıyla Azure kaynak grupları oluşturma ve dağıtma.](../azure-resource-manager/templates/create-visual-studio-deployment-project.md)

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir HDInsight kümesi oluşturmak için çeşitli yollar öğrendim. Daha fazla bilgi için aşağıdaki makalelere bakın:

* HDInsight ile ilgili daha fazla şablon için [Azure hızlı başlangıç şablonlarına](https://azure.microsoft.com/resources/templates/?term=hdinsight)bakın.
* .NET istemci kitaplığı aracılığıyla kaynak dağıtma örneği [için](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bkz.
* Bir uygulamayı dağıtmaya ilişkin ayrıntılı bir örnek için, [Azure'da tahmin edilebilir şekilde Sağlama ve mikro hizmetleri dağıtma bölümüne](../app-service/deploy-complex-application-predictably.md)bakın.
* Çözümünüzü farklı ortamlarda dağıtmaya yönelik kılavuz için bkz. [Microsoft Azure’da geliştirme ve test ortamları](../solution-dev-test-environments.md).
* Azure Kaynak Yöneticisi şablonunun bölümleri hakkında bilgi edinmek için [bkz.](../azure-resource-manager/templates/template-syntax.md)
* Azure Kaynak Yöneticisi şablonunda kullanabileceğiniz işlevlerin listesi için [Şablon işlevlerine](../azure-resource-manager/templates/template-functions.md)bakın.
