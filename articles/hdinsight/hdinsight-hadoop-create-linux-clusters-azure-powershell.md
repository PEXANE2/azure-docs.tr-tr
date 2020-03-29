---
title: PowerShell'i kullanarak Apache Hadoop kümeleri oluşturun - Azure HDInsight
description: Azure PowerShell'i kullanarak LINUX for HDInsight için Apache Hadoop, Apache HBase, Apache Storm veya Apache Spark kümelerini nasıl oluşturacağınız hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/18/2019
ms.openlocfilehash: 3c93eca493275612ac14a995140b2e91cc40fe98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644673"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Azure PowerShell'i kullanarak HDInsight'ta Linux tabanlı kümeler oluşturun

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell, Microsoft Azure'da iş yüklerinizin dağıtımını ve yönetimini denetlemek ve otomatikleştirmek için kullanabileceğiniz güçlü bir komut dosyası ortamıdır. Bu belge, Azure PowerShell'i kullanarak Linux tabanlı bir HDInsight kümesinin nasıl oluşturulabildiğini hakkında bilgi sağlar. Ayrıca bir örnek komut dosyası içerir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure PowerShell](/powershell/azure/install-Az-ps) Az modülü.

## <a name="create-cluster"></a>Küme oluşturma

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Azure PowerShell'i kullanarak bir HDInsight kümesi oluşturmak için aşağıdaki yordamları tamamlamanız gerekir:

* Azure kaynak grubu oluşturma
* Azure Depolama hesabı oluşturma
* Azure Blob kapsayıcısı oluşturma
* HDInsight kümesi oluşturma

> [!NOTE]
> Azure Veri Gölü Depolama Gen2 ile hdinsight kümesi oluşturmak için PowerShell'in kullanılması şu anda desteklenmez.

Aşağıdaki komut dosyası, yeni bir kümenin nasıl oluşturultur olduğunu gösterir:

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

Küme girişi için belirttiğiniz değerler küme için Hadoop kullanıcı hesabı oluşturmak için kullanılır. Web UI'leri veya REST API'leri gibi kümede barındırılan hizmetlere bağlanmak için bu hesabı kullanın.

SSH kullanıcısı için belirttiğiniz değerler küme için SSH kullanıcısını oluşturmak için kullanılır. Kümeüzerinde uzak bir SSH oturumu başlatmak ve işleri çalıştırmak için bu hesabı kullanın. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

> [!IMPORTANT]  
> 32'den fazla alt düğüm kullanmayı planlıyorsanız (küme oluşturmada veya oluşturulduktan sonra kümeyi ölçekleyerek), en az 8 çekirdekli ve 14 GB RAM'e sahip bir kafa düğümü boyutu da belirtmeniz gerekir.
>
> Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).

Bir küme oluşturmak 20 dakika kadar sürebilir.

## <a name="create-cluster-configuration-object"></a>Küme oluşturma: Yapılandırma nesnesi

Cmdlet kullanarak [`New-AzHDInsightClusterConfig`](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightclusterconfig) bir HDInsight yapılandırma nesnesi de oluşturabilirsiniz. Daha sonra kümeniz için ek yapılandırma seçeneklerini etkinleştirmek için bu yapılandırma nesnesini değiştirebilirsiniz. Son olarak, `-Config` yapılandırmayı [`New-AzHDInsightCluster`](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) kullanmak için cmdlet parametresini kullanın.

Aşağıdaki komut dosyası, HDInsight küme türünde bir R Server'ı yapılandırmak için bir yapılandırma nesnesi oluşturur. Yapılandırma bir kenar düğümü, RStudio ve ek bir depolama hesabı sağlar.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-99)]

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda bir depolama hesabı nın kullanılması desteklenmez. Bu örneği kullanırken, sunucuyla aynı konumda ek depolama hesabı oluşturun.

## <a name="customize-clusters"></a>Kümeleri özelleştirme

* [Bootstrap kullanarak HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell)bakın.
* [Bkz. Script Action'ı kullanarak HDInsight kümelerini özelleştirin.](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir HDInsight kümesini başarıyla oluşturduğunuza göre, kümenizle nasıl çalışacağınızı öğrenmek için aşağıdaki kaynakları kullanın.

### <a name="apache-hadoop-clusters"></a>Apaçi Hadoop kümeleri

* [HDInsight ile Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile MapReduce'ı kullanın](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri

* [HDInsight'ta Apache HBase ile başlayın](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight'ta Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Fırtına kümeleri

* [HDInsight'ta Storm için Java topolojileri geliştirin](storm/apache-storm-develop-java-topology.md)
* [HDInsight'ta Storm'da Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight'ta Storm ile topolojileri dağıtın ve izleyin](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Kıvılcım kümeleri

* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](spark/apache-spark-livy-rest-interface.md)
* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](spark/apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](spark/apache-spark-machine-learning-mllib-ipython.md)