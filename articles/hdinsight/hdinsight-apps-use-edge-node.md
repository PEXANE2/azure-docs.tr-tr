---
title: Azure HDInsight'ta Apache Hadoop kümelerinde boş kenar düğümleri kullanma
description: İstemci olarak kullanılabilen bir HDInsight kümesine boş kenar düğümü ekleme ve ardından HDInsight uygulamalarınızı test etme/barındırma.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 01/27/2020
ms.openlocfilehash: d7723ea63cbb9bab6adf42d7e92f84a6b8b2ab9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272610"
---
# <a name="use-empty-edge-nodes-on-apache-hadoop-clusters-in-hdinsight"></a>HDInsight'ta Apache Hadoop kümelerinde boş kenar düğümlerini kullanma

HDInsight kümesine boş kenar düğümü eklemeyi öğrenin. Boş kenar düğümü, headnodes olarak yüklü ve yapılandırılmış aynı istemci araçları ile bir Linux sanal makine, ancak hiçbir [Apache Hadoop](https://hadoop.apache.org/) hizmetleri çalışıyor. Kümeye erişmek, istemci uygulamalarınızı test etmek ve istemci uygulamalarınızı barındırmak için kenar düğümlerini kullanabilirsiniz.

Kümeyi oluştururken yeni bir kümeye varolan bir HDInsight kümesine boş kenar düğümü ekleyebilirsiniz. Boş kenar düğümü ekleme, Azure Kaynak Yöneticisi şablonu kullanılarak yapılır.  Aşağıdaki örnek, şablon kullanılarak nasıl yapıldığını gösterir:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "{}"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Örnekte gösterildiği gibi, kenar düğümüne [Apache Hue](hdinsight-hadoop-hue-linux.md) yükleme gibi ek yapılandırma gerçekleştirmek için isteğe bağlı olarak komut [dosyası eylemi](hdinsight-hadoop-customize-cluster-linux.md) çağırabilirsiniz. Komut dosyası eylem komut dosyası web'de herkese açık olmalıdır.  Örneğin, komut dosyası Azure Depolama'da depolanıyorsa, ortak kapsayıcılar veya ortak lekeler kullanın.

Kenar düğümü sanal makine boyutu HDInsight küme işçi düğümü vm boyut gereksinimlerini karşılamalıdır. Önerilen işçi düğümü vm boyutları için [HDInsight'ta Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md#cluster-type)bölümüne bakın.

Bir kenar düğümü oluşturduktan sonra, SSH kullanarak kenar düğümüne bağlanabilir ve HDInsight'ta Hadoop kümesine erişmek için istemci araçlarını çalıştırabilirsiniz.

> [!WARNING]
> Kenar düğümüne yüklenen özel bileşenler Microsoft'tan ticari olarak makul destek alır. Bu, karşılaştığınız sorunların çözülmesiyle sonuçlanabilir. Veya, daha fazla yardım için topluluk kaynaklarına yönlendirilebilirsiniz. Topluluktan yardım almak için en etkin sitelerden bazıları şunlardır:
>
> * [HDInsight için MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [https://stackoverflow.com](https://stackoverflow.com).
>
> Bir Apache teknolojisi kullanıyorsanız, [https://apache.org](https://apache.org) [Apache Hadoop](https://hadoop.apache.org/) sitesi gibi Apache proje siteleri aracılığıyla yardım bulabilirsiniz.

> [!IMPORTANT]
> Ubuntu görüntüleri yayınlandıktan sonra 3 ay içinde yeni HDInsight küme oluşturma için kullanılabilir hale gelir. Ocak 2019 itibariyle, çalışan kümeler (kenar düğümleri dahil) otomatik yamalı **değildir.** Müşteriler çalışan bir kümeyi yamalamak için komut dosyası eylemlerini veya diğer mekanizmaları kullanmalıdır.  Daha fazla bilgi [için HDInsight için işletim sistemi yaması 'na](./hdinsight-os-patching.md)bakın.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Varolan bir kümeye kenar düğümü ekleme

Bu bölümde, varolan bir HDInsight kümesine kenar düğümü eklemek için bir Kaynak Yöneticisi şablonu kullanırsınız.  Kaynak Yöneticisi şablonu [GitHub'da](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-add-edge-node/)bulunabilir. Kaynak Yöneticisi https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.shşablonu' nda bulunan bir komut dosyası eylemi çağırır. Komut dosyası herhangi bir eylem gerçekleştirmez.  Kaynak Yöneticisi şablonundan arama komut dosyası eylemini göstermek içindir.

1. Azure'da oturum açmak ve Azure portalında Azure Kaynak Yöneticisi şablonuna açmak için aşağıdaki resmi seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

1. Aşağıdaki özellikleri yapılandırın:

    |Özellik |Açıklama |
    |---|---|
    |Abonelik|Küme oluşturmak için kullanılan bir Azure aboneliği seçin.|
    |Kaynak grubu|Varolan HDInsight kümesi için kullanılan kaynak grubunu seçin.|
    |Konum|Varolan HDInsight kümesinin konumunu seçin.|
    |Küme Adı|Varolan bir HDInsight kümesinin adını girin.|

1. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**ve kenar düğümünü oluşturmak için Satın **Alma'yı** seçin.

> [!IMPORTANT]  
> Varolan HDInsight kümesi için Azure kaynak grubunu seçtiğinizden emin olun.  Aksi takdirde, "İç içe kaynakta istenen işlemi gerçekleştiremezsiniz hata iletisi alırsınız. Üst kaynak&lt;'ClusterName>' bulunamadı."

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Küme oluştururken kenar düğümü ekleme

Bu bölümde, kenar düğümü olan HDInsight kümesi oluşturmak için bir Kaynak Yöneticisi şablonu kullanırsınız.  Kaynak Yöneticisi şablonu Azure [hızlı başlatma şablonları galerisinde](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/)bulunabilir. Kaynak Yöneticisi https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.shşablonu' nda bulunan bir komut dosyası eylemi çağırır. Komut dosyası herhangi bir eylem gerçekleştirmez.  Kaynak Yöneticisi şablonundan arama komut dosyası eylemini göstermek içindir.

1. Henüz bir HDInsight kümeniz yoksa bir HDInsight kümesi oluşturun.  Bkz. [HDInsight'ta Hadoop'u kullanmaya başlayın.](hadoop/apache-hadoop-linux-tutorial-get-started.md)

1. Azure'da oturum açmak ve Azure portalında Azure Kaynak Yöneticisi şablonuna açmak için aşağıdaki resmi seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

1. Aşağıdaki özellikleri yapılandırın:

    |Özellik |Açıklama |
    |---|---|
    |Abonelik|Küme oluşturmak için kullanılan bir Azure aboneliği seçin.|
    |Kaynak grubu|Küme için kullanılan yeni bir kaynak grubu oluşturun.|
    |Konum|Kaynak grubu için bir konum seçin.|
    |Küme Adı|Oluşturmak için yeni küme için bir ad girin.|
    |Küme Oturum Açma Kullanıcı Adı|Hadoop HTTP kullanıcı adını girin.  Varsayılan ad, **admin** şeklindedir.|
    |Küme Oturum Açma Parolası|Hadoop HTTP kullanıcı parolasını girin.|
    |Ssh Kullanıcı Adı|SSH kullanıcı adını girin. Varsayılan ad **sshuser**olduğunu.|
    |Ssh Şifre|SSH kullanıcı parolasını girin.|
    |Komut Dosyası Eylemi Yükle|Bu makalede gezinmek için varsayılan değeri tutun.|

    Şablonda bazı özellikler kodlanmıştır: Küme türü, Küme alt düğümü sayısı, Kenar düğümü boyutu ve Kenar düğümü adı.

1. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**ve kenar düğümü ile küme oluşturmak için Satın **Alma'yı** seçin.

## <a name="add-multiple-edge-nodes"></a>Birden çok kenar düğümü ekleme

BIR HDInsight kümesine birden çok kenar düğümü ekleyebilirsiniz.  Birden çok kenar düğümleri yapılandırması yalnızca Azure Kaynak Yöneticisi Şablonları kullanılarak yapılabilir.  Bu makalenin başındaki şablon örneğine bakın.  Oluşturmak istediğiniz kenar düğümleri sayısını yansıtacak şekilde **hedefInstanceCount'ı** güncelleştirmeniz gerekir.

## <a name="access-an-edge-node"></a>Kenar düğümüne erişin

Kenar düğümü ssh bitiş &lt;noktası EdgeNodeName>. &lt;ClusterName>-ssh.azurehdinsight.net:22.  Örneğin, new-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Kenar düğümü Azure portalında bir uygulama olarak görünür.  Portal, SSH kullanarak kenar düğümüne erişmeniz için size bilgi verir.

**Kenar düğümü SSH bitiş noktasını doğrulamak için**

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. HDInsight kümesini kenar düğümüyle açın.
3. **Uygulamaları**seçin. Kenar düğümlerini göreceksiniz.  Varsayılan ad **yeni kenarlı düğümdür.**
4. Kenar düğümünü seçin. SSH bitiş noktasını göreceksiniz.

**Kenar düğümünde Kovan kullanmak için**

1. Kenar düğümüne bağlanmak için SSH kullanın. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. SSH kullanarak kenar düğümüne bağlandıktan sonra Kovan konsolunu açmak için aşağıdaki komutu kullanın:

        hive

3. Kümedeki Hive tablolarını göstermek için aşağıdaki komutu çalıştırın:

        show tables;

## <a name="delete-an-edge-node"></a>Kenar düğümünü silme

Azure portalından bir kenar düğümünü silebilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. HDInsight kümesini kenar düğümüyle açın.
3. **Uygulamaları**seçin. Kenar düğümlerinin bir listesini göreceksiniz.  
4. Silmek istediğiniz kenar düğümünü sağ tıklatın ve sonra **Sil'i**seçin.
5. Onaylamak için **Evet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kenar düğümü eklemeyi ve kenar düğümüne nasıl erişini öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md): HDInsight uygulamalarını kümelerinize yükleme hakkında bilgi alın.
* [Özel HDInsight uygulamalarını yükleyin:](hdinsight-apps-install-custom-applications.md)yayınlanmamış bir HDInsight uygulamasını HDInsight'a nasıl dağıtılayarak dağıtılamayı öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Linux tabanlı Apache Hadoop kümeleri oluşturun:](hdinsight-hadoop-create-linux-clusters-arm-templates.md)HDInsight kümeleri oluşturmak için Kaynak Yöneticisi şablonlarını nasıl çağıracağınız öğrenin.
