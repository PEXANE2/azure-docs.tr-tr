---
title: Verileri dönüştürme
description: Hadoop, Machine Learning veya Azure Data Lake Analytics'i kullanarak Azure Veri Fabrikası'nda verileri nasıl dönüştürdüğünüz veya verileri nasıl işittiğinizi öğrenin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: shwang
ms.custom: seo-lt-2019
ms.date: 07/31/2018
ms.openlocfilehash: 024d61c5bcc38c78dc5a2bd5e01cba43d7b5267e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418839"
---
# <a name="transform-data-in-azure-data-factory"></a>Azure Data Factory'de veri dönüştürme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

> [!div class="op_single_selector"]
> * [Veri akışını eşleme](data-flow-create.md)
> * [Hive](transform-data-using-hadoop-hive.md)  
> * [Domuz](transform-data-using-hadoop-pig.md)  
> * [MapReduce](transform-data-using-hadoop-map-reduce.md)  
> * [HDInsight Akış](transform-data-using-hadoop-streaming.md)
> * [HDInsight Spark](transform-data-using-spark.md)
> * [Makine Öğrenimi](transform-data-using-machine-learning.md) 
> * [Depolanan Yordam](transform-data-using-stored-procedure.md)
> * [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md)
> * [Databricks dizüstü bilgisayar](transform-data-databricks-notebook.md)
> * [Databricks Kavanoz](transform-data-databricks-jar.md)
> * [Databricks Python](transform-data-databricks-python.md)
> * [.NET özel](transform-data-using-dotnet-custom-activity.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure Veri Fabrikası'nda ham verilerinizi ölçekte öngörülere ve öngörülere dönüştürmek ve işlemek için kullanabileceğiniz veri dönüştürme faaliyetleri açıklanmaktadır. Bir dönüştürme etkinliği, Azure Databricks veya Azure HDInsight gibi bir bilgi işlem ortamında yürütülür. Her dönüşüm etkinliği hakkında ayrıntılı bilgi içeren makalelere bağlantılar sağlar.

Veri Fabrikası, [boru hatlarına](concepts-pipelines-activities.md) tek tek veya başka bir etkinlikle zincirlenebilecek aşağıdaki veri dönüştürme etkinliklerini destekler.

## <a name="transform-natively-in-azure-data-factory-with-data-flows"></a>Veri akışlarıyla Azure Veri Fabrikası'nda yerel olarak dönüştürme

### <a name="mapping-data-flows"></a>Veri akışlarını eşleme

Veri akışlarını haritalama, Azure Veri Fabrikası'nda görsel olarak tasarlanmış veri dönüşümleridir. Veri akışları, veri mühendislerinin kod yazmadan grafiksel veri dönüştürme mantığı geliştirmelerine olanak sağlar. Ortaya çıkan veri akışları, ölçeklenmiş Spark kümelerini kullanan Azure Veri Fabrikası ardışık hatlarındaki etkinlikler olarak yürütülür. Veri akışı faaliyetleri, mevcut Veri Fabrikası zamanlama, kontrol, akış ve izleme yetenekleri aracılığıyla operasyonel hale getirilebilir. Daha fazla bilgi için [veri akışlarını eşleme bilgisine](concepts-data-flow-overview.md)bakın.

### <a name="wrangling-data-flows"></a>Çekişmeli veri akışları

Azure Veri Fabrikası'ndaki veri akışlarının düzenlenmesi, bulut ölçeğinde yinelemeli olarak kodsuz veri hazırlama yapmanıza olanak tanır. Wrangling veri akışları [Power Query Online](https://docs.microsoft.com/power-query/) ile tümleşir ve Power Query M işlevlerini kıvılcım yürütme yoluyla bulut ölçeğinde veri çekişmesi için kullanılabilir hale getirir. Daha fazla bilgi için, [çekişmeli veri akışlarına](wrangling-data-flow-overview.md)bakın.

## <a name="external-transformations"></a>Dış dönüşümler

İsteğe bağlı olarak, dönüşümleri el koduyla kodlayabilir ve dış işlem ortamını kendiniz yönetebilirsiniz.

### <a name="hdinsight-hive-activity"></a>HDInsight Kovan etkinliği
Veri Fabrikası ardışık işlonundaki HDInsight Hive etkinliği, Kovan sorgularını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür. Bu etkinlikle ilgili ayrıntılar için [Kovan etkinlik](transform-data-using-hadoop-hive.md) makalesine bakın. 

### <a name="hdinsight-pig-activity"></a>HDInsight Pig etkinliği
Veri Fabrikası ardışık işakindeki HDInsight Pig etkinliği, Pig sorgularını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür. Bu etkinlikle ilgili ayrıntılar için [Pig etkinlik](transform-data-using-hadoop-pig.md) makalesine bakın. 

### <a name="hdinsight-mapreduce-activity"></a>HDInsight MapEtkinliği azaltın
Bir Veri Fabrikası boru hattındaki HDInsight MapReduce etkinliği, MapReduce programlarını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür. Bu etkinlikle ilgili ayrıntılar için [MapReduce etkinlik](transform-data-using-hadoop-map-reduce.md) makalesine bakın.

### <a name="hdinsight-streaming-activity"></a>HDInsight Akış etkinliği
Veri Fabrikası boru hattındaki HDInsight Akış etkinliği, Hadoop Streaming programlarını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür. Bu etkinlikle ilgili ayrıntılar için [HDInsight Streaming etkinliğine](transform-data-using-hadoop-streaming.md) bakın.

### <a name="hdinsight-spark-activity"></a>HDInsight Spark etkinliği
Veri Fabrikası ardışık işakindeki HDInsight Spark etkinliği, Spark programlarını kendi HDInsight kümenizde yürütür. Ayrıntılar için Azure [Veri Fabrikası'ndan Çağrı Spark programlarına](transform-data-using-spark.md)bakın. 

### <a name="machine-learning-activities"></a>Makine Öğrenimi etkinlikleri
Azure Veri Fabrikası, tahmine dayalı analitik için yayınlanmış bir Azure Machine Learning web hizmetini kullanan ardışık hatlar oluşturmanıza olanak tanır. Azure Veri Fabrikası ardışık bir ardışık ardışık [ardışık toplu işlem etkinliğini](transform-data-using-machine-learning.md) kullanarak, toplu olarak veriler üzerinde öngörülerde bulunmak için bir Machine Learning web hizmetini çağırabilirsiniz.

Zaman içinde, Machine Learning puanlama deneylerinde tahmine dayalı modellerin yeni giriş veri kümeleri kullanılarak yeniden eğitilmesi gerekir. Yeniden eğitim bittikten sonra, yeniden eğitilen Machine Learning modeli yle puanlama web hizmetini güncelleştirmek istersiniz. Web hizmetini yeni eğitilmiş modelle güncelleştirmek için [Kaynağı Güncelleştir etkinliğini](update-machine-learning-models.md) kullanabilirsiniz.  

Bu Makine Öğrenimi etkinlikleri hakkında ayrıntılı bilgi için [Makine Öğrenimi etkinliklerini kullanın.](transform-data-using-machine-learning.md) 

### <a name="stored-procedure-activity"></a>Depolanan yordam etkinliği
Aşağıdaki veri depolarından birinde depolanan bir yordamı çağırmak için Veri Fabrikası ardışık ardışık işlemdeki SQL Server Stored Yordam etkinliğini kullanabilirsiniz: Azure SQL Veritabanı, Azure SQL Veri Ambarı, kuruluşunuzdaki SQL Server Veritabanı veya Azure VM. Ayrıntılar için [Depolanan Yordam etkinlik](transform-data-using-stored-procedure.md) makalesine bakın.  

### <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL etkinliği
Data Lake Analytics U-SQL etkinliği, Azure Veri Gölü Analizi kümesinde bir U-SQL komut dosyası çalıştırıyor. Ayrıntılar için [Veri Analizi U-SQL etkinlik](transform-data-using-data-lake-analytics.md) makalesine bakın. 

### <a name="databricks-notebook-activity"></a>Databricks Notebook etkinliği

Veri Fabrikası ardışık alanındaki Azure Databricks Dizüstü Etkinliği, Azure Databricks çalışma alanınızda bir Databricks dizüstü bilgisayarı çalıştırıyor. Azure Databricks, Apache Spark'ı çalıştırmak için yönetilen bir platformdur. Bkz. [Bir Databricks not defteri çalıştırarak verileri dönüştür.](transform-data-databricks-notebook.md)

### <a name="databricks-jar-activity"></a>Databricks Jar etkinliği

Veri Fabrikası ardışık işakindeki Azure Veri Tuğlaları Jar Etkinliği, Azure Veri Tuğlaları kümenizde bir Kıvılcım Kavanozu çalıştırAr. Azure Databricks, Apache Spark'ı çalıştırmak için yönetilen bir platformdur. Bkz. [Azure Veri Tuğlaları'nda bir Jar etkinliği çalıştırarak verileri dönüştürün.](transform-data-databricks-jar.md)

### <a name="databricks-python-activity"></a>Databricks Python etkinliği

Veri Fabrikası ardışık dizisindeki Azure Veri Tuğlaları Python Etkinliği, Azure Veri Tuğlaları kümenizde bir Python dosyasını çalıştırAr. Azure Databricks, Apache Spark'ı çalıştırmak için yönetilen bir platformdur. Bkz. [Azure Veri Tuğlaları'nda bir Python etkinliği çalıştırarak verileri dönüştürün.](transform-data-databricks-python.md)

### <a name="custom-activity"></a>Özel etkinlik
Verileri Veri Fabrikası tarafından desteklenmeyen bir şekilde dönüştürmeniz gerekiyorsa, kendi veri işleme mantığınızla özel bir etkinlik oluşturabilir ve ardışık işlemde etkinliği kullanabilirsiniz. Özel .NET etkinliğini, Azure Toplu İş hizmeti veya Azure HDInsight kümesini kullanarak çalışacak şekilde yapılandırabilirsiniz. Bkz. Ayrıntılar için [özel etkinlikler makalekullan.](transform-data-using-dotnet-custom-activity.md) 

R yüklü HDInsight kümenizde R betiklerini çalıştırmak için özel bir etkinlik oluşturabilirsiniz. Bkz. [Azure Data Factory kullanarak R Betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample). 

### <a name="compute-environments"></a>İşlem ortamları
İşlem ortamı için bağlantılı bir hizmet oluşturursunuz ve bir dönüşüm etkinliği tanımlarken bağlantılı hizmeti kullanırsınız. Veri Fabrikası tarafından desteklenen iki tür bilgi işlem ortamı vardır. 

- **İsteğe Bağlı**: Bu durumda, bilgi işlem ortamı tamamen Veri Fabrikası tarafından yönetilir. Bir iş işlem verilerine gönderilmeden ve iş tamamlandığında kaldırılmadan önce Veri Fabrikası hizmeti tarafından otomatik olarak oluşturulur. İş yürütme, küme yönetimi ve önyükleme eylemleri için isteğe bağlı bilgi işlem ortamının parçalı ayarlarını yapılandırabilir ve denetleyebilirsiniz. 
- **Kendi Getirin**: Bu durumda, kendi bilgi işlem ortamınızı (örneğin HDInsight kümesi) Veri Fabrikası'nda bağlantılı bir hizmet olarak kaydedebilirsiniz. Bilgi işlem ortamı sizin tarafından yönetilir ve Veri Fabrikası hizmeti bunu etkinlikleri yürütmek için kullanır. 

Veri Fabrikası tarafından desteklenen bilgi işlem hizmetleri hakkında bilgi edinmek için Bağlantılı [Hizmetleri İşlem](compute-linked-services.md) eki makaleye bakın. 

## <a name="next-steps"></a>Sonraki adımlar
Dönüşüm etkinliği kullanma örneği için aşağıdaki öğreticiye bakın: [Öğretici: Spark kullanarak verileri dönüştürme](tutorial-transform-data-spark-powershell.md)
