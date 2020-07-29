---
title: HDInsight 'ta Hadoop ile Windows BILGISAYARı kullanma-Azure
description: HDInsight 'ta Hadoop 'ta bir Windows BILGISAYARDAN çalışın. PowerShell, Visual Studio ve Linux araçlarıyla kümeleri yönetin ve sorgulayın. .NET ile büyük veri çözümleri geliştirin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 12/20/2019
ms.openlocfilehash: 3ec50acc693452fe73d929effcea98b12fc5ff8b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75933945"
---
# <a name="work-in-the-apache-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Windows BILGISAYARDAN HDInsight üzerinde Apache Hadoop ekosisteminde çalışma

HDInsight üzerinde Apache Hadoop ekosisteminde çalışmak üzere Windows bilgisayarında geliştirme ve yönetim seçenekleri hakkında bilgi edinin.

HDInsight, Linux üzerinde geliştirilen Apache Hadoop ve Hadoop bileşenlerine, açık kaynaklı teknolojilere dayalıdır. HDInsight sürüm 3,4 ve üzeri, küme için temel alınan işletim sistemi olarak Ubuntu Linux dağıtımını kullanır. Ancak, Windows istemcisi veya Windows geliştirme ortamından HDInsight ile çalışabilirsiniz.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Dağıtım ve yönetim görevleri için PowerShell 'i kullanma

Azure PowerShell, Windows 'dan HDInsight 'ta dağıtım ve yönetim görevlerini denetlemek ve otomatikleştirmek için kullanabileceğiniz bir betik ortamıdır.

PowerShell ile gerçekleştirebileceğiniz görev örnekleri:

* [PowerShell kullanarak kümeler oluşturun](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).
* [PowerShell kullanarak Apache Hive sorguları çalıştırın](hadoop/apache-hadoop-use-hive-powershell.md).
* [Kümeleri PowerShell Ile yönetin](hdinsight-administer-use-powershell.md).

En son sürümü almak için [Azure PowerShell 'i yüklemek ve yapılandırmak](https://docs.microsoft.com/powershell/azure/install-az-ps) için adımları izleyin.

## <a name="utilities-you-can-run-in-a-browser"></a>Bir tarayıcıda çalıştırabileceğiniz yardımcı programlar

Aşağıdaki yardımcı programlar bir tarayıcıda çalışan bir Web Kullanıcı arabirimine sahiptir:
* **[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)** , tarayıcınızda ve Azure Portal içinden çalışan etkileşimli, komut satırı kabuğudur.

* **[Apache ambarı Web Kullanıcı arabirimi](hdinsight-hadoop-manage-ambari.md)** , farklı türlerde işleri yönetmek için kullanılabilecek Azure Portal kullanılabilen bir yönetim ve izleme yardımcı programıdır:
    * [REST API ile Apache ambarı 'nı kullanma](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Apache ambarı 'nda Apache Hive görünümü](hadoop/apache-hadoop-use-hive-ambari-view.md)
    * [Apache ambarı 'nda Apache Tez görünümü](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Visual Studio için Data Lake (Hadoop) araçları

Fırtınası topolojilerini dağıtmak ve yönetmek için Visual Studio Data Lake araçları 'nı kullanın. Data Lake araçları, Visual Studio ile C# fırtınası topolojileri geliştirmenize olanak sağlayan SCP.NET SDK 'sını de yüklüyor.

Aşağıdaki örneklere geçmeden önce, [Visual Studio için Data Lake Araçları ' nı yükleyip deneyin](hadoop/apache-hadoop-visual-studio-tools-get-started.md).

Visual Studio ile gerçekleştirebileceğiniz görev örnekleri ve Visual Studio için Data Lake araçları:
* [Visual Studio 'dan fırtınası topolojilerini dağıtma ve yönetme](storm/apache-storm-deploy-monitor-topology-linux.md)
* [Visual Studio kullanarak fırtınası Için C# topolojileri geliştirin](storm/apache-storm-develop-csharp-visual-studio-topology.md). BITS, Azure Cosmos DB ve SQL veritabanı gibi veritabanlarına bağlanabilmeniz için, fırtınası Topolojilerine yönelik örnek şablonlar içerir.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio ve .NET SDK

Kümeleri yönetmek ve büyük veri uygulamaları geliştirmek için Visual Studio 'Yu .NET SDK ile birlikte kullanabilirsiniz. Aşağıdaki görevler için diğer Ides 'i kullanabilirsiniz, ancak örnekler Visual Studio 'da gösterilir.

Visual Studio 'da .NET SDK ile gerçekleştirebileceğiniz görev örnekleri:
* [.Net Için Azure HDıNSIGHT SDK](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet).
* [.NET SDK kullanarak Apache Hive sorguları çalıştırın](hadoop/apache-hadoop-use-hive-dotnet-sdk.md).
* [Apache Hadoop üzerinde Apache Hive ve Apache Pig streaming Ile C# Kullanıcı tanımlı Işlevleri kullanın](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Spark kümeleri için IntelliJ fıkır ve tutulma IDE

Hem [IntelliJ fikri](https://www.jetbrains.com/idea/download) hem de [tutulma IDE](https://www.eclipse.org/downloads/) 'si şu şekilde kullanılabilir:
* Bir HDInsight Spark kümesi üzerinde bir Scala Spark uygulaması geliştirin ve gönderebilirsiniz.
* Spark kümesi kaynaklarına erişin.
* Yerel olarak bir Scala Spark uygulaması geliştirin ve çalıştırın.

Bu makaleler şunları göstermektedir:
* IntelliJ fıkır: [IntelliJ eklentisi ve Scala SDK Için Azure araç seti 'ni kullanarak Apache Spark uygulamalar oluşturun.](spark/apache-spark-intellij-tool-plugin.md)
* Çakışan Küreler IDE veya Scala IDE: [Apache Spark uygulamalar ve Azure Toolkit for Eclipse oluşturun](spark/apache-spark-eclipse-tool-plugin.md)

## <a name="notebooks-on-spark-for-data-scientists"></a>Veri bilimcileri için Spark 'ta Not defterleri

HDInsight 'ta Apache Spark kümeleri, Jupyter Not defterleri ile kullanılabilen Apache Zeppelin not defterlerini ve çekirdekler 'leri içerir.

* [Spark uygulamalarını test etmek için Jupyter Not defteri ile Apache Spark kümelerinde çekirdekler 'yi nasıl kullanacağınızı öğrenin](spark/apache-spark-zeppelin-notebook.md)
* [Spark işlerini çalıştırmak için Apache Spark kümelerinde Apache Zeppelin not defterlerini nasıl kullanacağınızı öğrenin](spark/apache-spark-jupyter-notebook-kernels.md)

## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Windows 'da Linux tabanlı araçları ve teknolojileri çalıştırma

Yalnızca Linux 'ta kullanılabilen bir araç veya teknolojiyi kullanmanız gereken bir durumda geliyorsa, aşağıdaki seçenekleri göz önünde bulundurun:

* **Windows 10 üzerinde Ubuntu 'Da Bash** , Windows 'Ta bir Linux alt sistemi sağlar. Bash, adanmış bir Linux yüklemesini sürdürmenize gerek kalmadan doğrudan Linux yardımcı programlarını çalıştırmanızı sağlar. Yükleme adımları için bkz. [Windows 10 Linux Için Windows alt sistemi yükleme kılavuzu](https://docs.microsoft.com/windows/wsl/install-win10) .  Diğer [UNIX kabukları](https://www.gnu.org/software/bash/) de çalışacaktır.
* **Docker for Windows** , Linux tabanlı birçok araca erişim sağlar ve doğrudan Windows 'tan çalıştırılabilir. Örneğin, Docker kullanarak Hive için Beeline istemcisini doğrudan Windows 'tan çalıştırabilirsiniz. Docker 'ı yerel bir Jupyter Not defteri çalıştırmak ve HDInsight 'ta Spark 'a uzaktan bağlanmak için de kullanabilirsiniz. [Docker for Windows kullanmaya başlayın](https://docs.docker.com/docker-for-windows/)
* **[MobaXterm](https://mobaxterm.mobatek.net/)** , bir SSH bağlantısı üzerinden küme dosya sistemine grafik olarak göz atmanıza olanak sağlar.

## <a name="cross-platform-tools"></a>Platformlar arası Araçlar

Azure komut satırı arabirimi (CLI), Azure kaynaklarını yönetmek için Microsoft tarafından sunulan platformlar arası komut satırı deneyimidir.  Daha fazla bilgi için bkz. [Azure komut satırı arabirimi (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="next-steps"></a>Sonraki adımlar

Linux tabanlı kümelerde çalışmaya yeni başladıysanız, aşağıdaki makalelere bakın:
* [Apache Hadoop, Apache Kafka, Apache Spark veya diğer kümeleri ayarlama](hdinsight-hadoop-provision-linux-clusters.md)
* [Linux 'ta HDInsight kümelerine yönelik ipuçları](hdinsight-hadoop-linux-information.md)
