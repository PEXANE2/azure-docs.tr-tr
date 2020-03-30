---
title: HDInsight'ta Hadoop'lu bir Windows PC kullanma - Azure
description: HDInsight'ta Hadoop'taki bir Windows PC'den çalışın. PowerShell, Visual Studio ve Linux araçlarıyla kümeleri yönetin ve sorgula. .NET ile büyük veri çözümleri geliştirin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 12/20/2019
ms.openlocfilehash: 3ec50acc693452fe73d929effcea98b12fc5ff8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933945"
---
# <a name="work-in-the-apache-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Windows PC'den HDInsight'ta Apache Hadoop ekosisteminde çalışın

HDInsight'ta Apache Hadoop ekosisteminde çalışmak için Windows PC'deki geliştirme ve yönetim seçenekleri hakkında bilgi edinin.

HDInsight, Linux üzerinde geliştirilen açık kaynak teknolojileri olan Apache Hadoop ve Hadoop bileşenlerine dayanmaktadır. HDInsight sürüm 3.4 ve üzeri kümeiçin temel işletim sistemi olarak Ubuntu Linux dağıtımını kullanır. Ancak, bir Windows istemcisinden veya Windows geliştirme ortamından HDInsight ile çalışabilirsiniz.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Dağıtım ve yönetim görevleri için PowerShell'i kullanma

Azure PowerShell, Windows'tan HDInsight'ta dağıtım ve yönetim görevlerini denetlemek ve otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır.

PowerShell ile yapabileceğiniz görevlere örnekler:

* [PowerShell kullanarak kümeler oluşturun.](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [PowerShell'i kullanarak Apache Hive sorgularını çalıştırın.](hadoop/apache-hadoop-use-hive-powershell.md)
* [PowerShell ile kümeleri yönetin.](hdinsight-administer-use-powershell.md)

En son sürümü almak için [Azure Powershell'i yüklemek ve yapılandırmak](https://docs.microsoft.com/powershell/azure/install-az-ps) için gereken adımları izleyin.

## <a name="utilities-you-can-run-in-a-browser"></a>Tarayıcıda çalıştırabileceğiniz yardımcı programlar

Aşağıdaki yardımcı programların tarayıcıda çalışan bir web web arama sürümü vardır:
* **[Azure Bulut Kabuğu,](https://docs.microsoft.com/azure/cloud-shell/overview)** tarayıcınızda ve Azure portalı içinde çalışan etkileşimli, komut satırı kabuğudur.

* **[Apache Ambari Web Kullanıcı Arabirimi,](hdinsight-hadoop-manage-ambari.md)** Azure portalında bulunan ve aşağıdakiler gibi farklı işleri yönetmek için kullanılabilecek bir yönetim ve izleme programıdır:
    * [REST API ile Apache Ambari kullanın](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Apache Ambari'de Apache Hive Görünümü](hadoop/apache-hadoop-use-hive-ambari-view.md)
    * [Apaçi Ambari'de Apaçi Tez Görünümü](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Visual Studio için Veri Gölü (Hadoop) Araçları

Storm topolojilerini dağıtmak ve yönetmek için Visual Studio için Veri Gölü Araçlarını kullanın. Data Lake Tools ayrıca Visual Studio ile C# Storm topolojileri geliştirmenize olanak tanıyan SCP.NET SDK'yı da yükler.

Aşağıdaki örneklere geçmeden önce [Visual Studio için Veri Gölü Araçları'nı yükleyin ve deneyin.](hadoop/apache-hadoop-visual-studio-tools-get-started.md)

Visual Studio ve Visual Studio için Veri Gölü Araçları ile yapabileceğiniz görevlere örnekler:
* [Visual Studio'dan Fırtına topolojilerini dağıtın ve yönetin](storm/apache-storm-deploy-monitor-topology-linux.md)
* [Visual Studio kullanarak Storm için C # topolojileri geliştirin.](storm/apache-storm-develop-csharp-visual-studio-topology.md) Bitler, Azure Cosmos DB ve SQL Veritabanı gibi veritabanlarına bağlanabileceğiniz Fırtına topolojileri için örnek şablonlar içerir.

## <a name="visual-studio-and-the-net-sdk"></a>Görsel Stüdyo ve .NET SDK

Kümeleri yönetmek ve büyük veri uygulamaları geliştirmek için .NET SDK ile Visual Studio'yu kullanabilirsiniz. Aşağıdaki görevler için diğer IID'leri kullanabilirsiniz, ancak örnekler Visual Studio'da gösterilir.

Visual Studio'da .NET SDK ile yapabileceğiniz görevlere örnekler:
* [.NET için Azure HDInsight SDK.](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet)
* [.NET SDK'yı kullanarak Apache Hive sorgularını çalıştırın.](hadoop/apache-hadoop-use-hive-dotnet-sdk.md)
* [Apache Hadoop'ta Apache Hive ve Apache Pig akışı ile C# kullanıcı tanımlı işlevleri kullanın.](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Kıvılcım kümeleri için Intellij IDEA ve Eclipse IDE

Hem [Intellij IDEA](https://www.jetbrains.com/idea/download) ve [Eclipse IDE](https://www.eclipse.org/downloads/) için kullanılabilir:
* HDInsight Spark kümesinde bir Scala Spark uygulaması geliştirin ve gönderin.
* Spark küme kaynaklarına erişin.
* Yerel olarak bir Scala Spark uygulaması geliştirin ve çalıştırın.

Bu makaleler nasıl göstereceğim:
* Intellij IDEA: [Intellij eklentisi için Azure Araç Seti ve Scala SDK'yı kullanarak Apache Spark uygulamaları oluşturun.](spark/apache-spark-intellij-tool-plugin.md)
* Eclipse için Eclipse IDE veya Scala IDE: [Apache Spark uygulamaları ve Eclipse için Azure Araç Seti oluşturun](spark/apache-spark-eclipse-tool-plugin.md)

## <a name="notebooks-on-spark-for-data-scientists"></a>Veri bilimciler için Kıvılcım'da dizüstü bilgisayarlar

HDInsight'taki Apache Spark kümeleri, Jupyter dizüstü bilgisayarlarda kullanılabilen Apache Zeppelin dizüstü bilgisayarları ve çekirdekleri içerir.

* [Spark uygulamalarını test etmek için Jupyter dizüstü bilgisayarlarla Apache Spark kümelerinde çekirdekleri nasıl kullanacağınızı öğrenin](spark/apache-spark-zeppelin-notebook.md)
* [Spark işlerini çalıştırmak için Apache Zeppelin dizüstü bilgisayarlarını Apache Spark kümelerinde nasıl kullanacağınızı öğrenin](spark/apache-spark-jupyter-notebook-kernels.md)

## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Windows'da Linux tabanlı araçlar ve teknolojiler çalıştırın

Yalnızca Linux'ta kullanılabilen bir aracı veya teknolojiyi kullanmanız gereken bir durumla karşılaşırsanız, aşağıdaki seçenekleri göz önünde bulundurun:

* **Windows 10'da Ubuntu'daki Bash,** Windows'da bir Linux alt sistemi sağlar. Bash, özel bir Linux yüklemesini sürdürmek zorunda kalmadan doğrudan Linux yardımcı programları çalıştırmanızı sağlar. Yükleme adımları [için Windows 10 için Linux Yükleme Kılavuzu](https://docs.microsoft.com/windows/wsl/install-win10) için Windows Alt Sistemi'ne bakın.  Diğer [Unix kabukları](https://www.gnu.org/software/bash/) da çalışacaktır.
* **Docker for Windows** birçok Linux tabanlı araçlara erişim sağlar ve doğrudan Windows'dan çalıştırılabilir. Örneğin, Doğrudan Windows'tan Hive için Beeline istemcisini çalıştırmak için Docker'ı kullanabilirsiniz. Yerel bir Jupyter dizüstü bilgisayar çalıştırmak ve HDInsight'ta Spark'a uzaktan bağlanmak için Docker'ı da kullanabilirsiniz. [Windows için Docker ile başlayın](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm,](https://mobaxterm.mobatek.net/)** bir SSH bağlantısı üzerinden küme dosyası sistemine grafik olarak göz atmanızı sağlar.

## <a name="cross-platform-tools"></a>Çapraz platform araçları

Azure komut satırı arabirimi (CLI), Azure kaynaklarını yönetmek için Microsoft tarafından sunulan platformlar arası komut satırı deneyimidir.  Daha fazla bilgi için Azure [Komut Satırı Arabirimi (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Linux tabanlı kümelerde çalışmaya yeni başladıysanız, aşağıdaki makalelere bakın:
* [Apache Hadoop, Apache Kafka, Apache Spark veya diğer kümeleri ayarlama](hdinsight-hadoop-provision-linux-clusters.md)
* [Linux'ta HDInsight kümeleri için ipuçları](hdinsight-hadoop-linux-information.md)
