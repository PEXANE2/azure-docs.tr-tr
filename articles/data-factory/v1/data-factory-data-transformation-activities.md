---
title: 'Veri Dönüşümü: Verileri dönüştürme & işleme '
description: Hadoop, Machine Learning veya Azure Data Lake Analytics'i kullanarak Azure Veri Fabrikası'nda verileri nasıl dönüştürdüğünüz veya verileri nasıl işittiğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 5b3e2db9b9769dee7599a2446b272e04cc0bedf7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74703384"
---
# <a name="transform-data-in-azure-data-factory"></a>Azure Data Factory'de veri dönüştürme
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Domuz](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Hadoop Akışı](data-factory-hadoop-streaming-activity.md)
> * [Makine Öğrenimi](data-factory-azure-ml-batch-execution-activity.md) 
> * [Depolanan Yordam](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> * [.NET özel](data-factory-use-custom-activities.md)

## <a name="overview"></a>Genel Bakış
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki veri dönüştürme etkinliklerine](../transform-data.md)bakın.

Bu makalede, Azure Veri Fabrikası'nda ham verilerinizi öngörülere ve öngörülere dönüştürmek ve işlemek için kullanabileceğiniz veri dönüştürme faaliyetleri açıklanmaktadır. Dönüştürme etkinliği, Azure HDInsight kümesi veya Azure Toplu İş ihdası gibi bir bilgi işlem ortamında yürütülür. Her dönüşüm etkinliği hakkında ayrıntılı bilgi içeren makalelere bağlantılar sağlar.

Veri Fabrikası, [boru hatlarına](data-factory-create-pipelines.md) tek tek veya başka bir etkinlikle zincirlenebilecek aşağıdaki veri dönüştürme etkinliklerini destekler.

> [!NOTE]
> Adım adım yönergeleri içeren bir yol için [bkz.](data-factory-build-your-first-pipeline.md)  
> 
> 

## <a name="hdinsight-hive-activity"></a>HDInsight Kovan etkinliği
Veri Fabrikası ardışık işlonundaki HDInsight Hive etkinliği, Kovan sorgularını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür. Bu etkinlikle ilgili ayrıntılar için [Kovan Etkinliği](data-factory-hive-activity.md) makalesine bakın. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig etkinliği
Veri Fabrikası ardışık işakindeki HDInsight Pig etkinliği, Pig sorgularını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür. Bu etkinlikle ilgili ayrıntılar için [Domuz Etkinliği](data-factory-pig-activity.md) makalesine bakın. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapEtkinliği azaltın
Bir Veri Fabrikası boru hattındaki HDInsight MapReduce etkinliği, MapReduce programlarını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür. Bu etkinlikle ilgili ayrıntılar için [MapReduce Etkinlik](data-factory-map-reduce.md) makalesine bakın.

## <a name="hdinsight-streaming-activity"></a>HDInsight Akış etkinliği
Veri Fabrikası boru hattındaki HDInsight Akış Etkinliği, Hadoop Streaming programlarını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür. Bu etkinlikle ilgili ayrıntılar için [HDInsight Streaming etkinliğine](data-factory-hadoop-streaming-activity.md) bakın.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark Etkinliği
Veri Fabrikası ardışık işakindeki HDInsight Spark etkinliği, Spark programlarını kendi HDInsight kümenizde yürütür. Ayrıntılar için Azure [Veri Fabrikası'ndan Çağrı Spark programlarına](data-factory-spark.md)bakın. 

## <a name="machine-learning-activities"></a>Makine Öğrenimi etkinlikleri
Azure Veri Fabrikası, tahmine dayalı analitik için yayınlanmış bir Azure Machine Learning web hizmetini kullanan ardışık hatlar oluşturmanıza olanak tanır. Toplu [İşlem Yürütme Etkinliğini](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) bir Azure Veri Fabrikası ardışık hattında kullanarak, toplu olarak veriler üzerinde öngörülerde bulunmak için bir Machine Learning web hizmetini çağırabilirsiniz.

Zaman içinde, Machine Learning puanlama deneylerinde tahmine dayalı modellerin yeni giriş veri kümeleri kullanılarak yeniden eğitilmesi gerekir. Yeniden eğitim bittikten sonra, yeniden eğitilen Machine Learning modeli yle puanlama web hizmetini güncelleştirmek istersiniz. Web hizmetini yeni eğitilmiş modelle güncelleştirmek için [Kaynak Etkinliğini Güncelleştir'i](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) kullanabilirsiniz.  

Bu Makine Öğrenimi etkinlikleri hakkında ayrıntılı bilgi için [Makine Öğrenimi etkinliklerini kullanın.](data-factory-azure-ml-batch-execution-activity.md) 

## <a name="stored-procedure-activity"></a>Depolanan yordam etkinliği
Aşağıdaki veri depolarından birinde depolanan bir yordamı çağırmak için Veri Fabrikası ardışık ardışık işlemdeki SQL Server Stored Yordam etkinliğini kullanabilirsiniz: Azure SQL Veritabanı, Azure SQL Veri Ambarı, kuruluşunuzdaki SQL Server Veritabanı veya Azure VM. Ayrıntılar için [Depolanan Yordam Etkinliği](data-factory-stored-proc-activity.md) makalesine bakın.  

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL etkinliği
Data Lake Analytics U-SQL Etkinliği, Azure Veri Gölü Analizi kümesinde bir U-SQL komut dosyası çalıştırıyor. Ayrıntılar için [Veri Analizi U-SQL Etkinliği](data-factory-usql-activity.md) makalesine bakın. 

## <a name="net-custom-activity"></a>.NET özel etkinliği
Verileri Veri Fabrikası tarafından desteklenmeyen bir şekilde dönüştürmeniz gerekiyorsa, kendi veri işleme mantığınızla özel bir etkinlik oluşturabilir ve ardışık işlemde etkinliği kullanabilirsiniz. Özel .NET etkinliğini, Azure Toplu İş hizmeti veya Azure HDInsight kümesini kullanarak çalışacak şekilde yapılandırabilirsiniz. Bkz. Ayrıntılar için [özel etkinlikler makalekullan.](data-factory-use-custom-activities.md) 

R yüklü HDInsight kümenizde R betiklerini çalıştırmak için özel bir etkinlik oluşturabilirsiniz. Bkz. [Azure Data Factory kullanarak R Betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>İşlem ortamları
İşlem ortamı için bağlantılı bir hizmet oluşturursunuz ve bir dönüşüm etkinliği tanımlarken bağlantılı hizmeti kullanırsınız. Veri Fabrikası tarafından desteklenen iki tür bilgi işlem ortamı vardır. 

1. **İsteğe Bağlı**: Bu durumda, bilgi işlem ortamı tamamen Veri Fabrikası tarafından yönetilir. Bir iş işlem verilerine gönderilmeden ve iş tamamlandığında kaldırılmadan önce Veri Fabrikası hizmeti tarafından otomatik olarak oluşturulur. İş yürütme, küme yönetimi ve önyükleme eylemleri için isteğe bağlı bilgi işlem ortamının parçalı ayarlarını yapılandırabilir ve denetleyebilirsiniz. 
2. **Kendi Getirin**: Bu durumda, kendi bilgi işlem ortamınızı (örneğin HDInsight kümesi) Veri Fabrikası'nda bağlantılı bir hizmet olarak kaydedebilirsiniz. Bilgi işlem ortamı sizin tarafından yönetilir ve Veri Fabrikası hizmeti bunu etkinlikleri yürütmek için kullanır. 

Veri Fabrikası tarafından desteklenen bilgi işlem hizmetleri hakkında bilgi edinmek için Bağlantılı [Hizmetleri İşlem](data-factory-compute-linked-services.md) eki makaleye bakın. 

## <a name="summary"></a>Özet
Azure Veri Fabrikası, etkinlikler için aşağıdaki veri dönüştürme etkinliklerini ve bilgi işlem ortamlarını destekler. Dönüştürme etkinlikleri, tek tek veya başka bir etkinlikle zincirlenmiş boru hatlarına eklenebilir.

| Veri dönüştürme etkinliği | İşlem ortamı |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Domuz](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Akışı](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Depolanan Yordam](data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Veri Ambarı veya SQL Server |
| [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [Dotnet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] veya Azure Batch |

