---
title: HDInsight'ta ML Hizmetleri için Azure depolama çözümleri - Azure
description: HDInsight'ta ML Hizmetleri ile sunulan farklı depolama seçenekleri hakkında bilgi edinin
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/02/2020
ms.openlocfilehash: 1c79d0390a80a1358ddb09707fbabf6a5a2affdc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75660248"
---
# <a name="azure-storage-solutions-for-ml-services-on-azure-hdinsight"></a>Azure HDInsight'ta ML Hizmetleri için Azure depolama çözümleri

HDInsight'taki ML Hizmetleri, analiz sonuçları içeren verileri, kodu veya nesneleri kalıcı olarak sürdürmek için farklı depolama çözümleri kullanabilir. Bu çözümler aşağıdaki seçenekleri içerir:

- [Azure Blob](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)
- [Azure Dosya depolama](https://azure.microsoft.com/services/storage/files/)

HDInsight kümenizle birden fazla Azure depolama hesabına veya kapsayıcısına erişme seçeneğiniz de vardır. Azure Dosya depolama, kenar düğümünde kullanmak için kullanışlı bir veri depolama seçeneğidir ve örneğin Linux dosya sistemine Azure depolama dosyası paylaşımı nı monte etmenizi sağlar. Ancak Azure Dosyası paylaşımları, Windows veya Linux gibi desteklenen bir işletim sistemine sahip herhangi bir sistem tarafından monte edilebilir ve kullanılabilir.

HDInsight'ta bir Apache Hadoop kümesi oluşturduğunuzda, bir **Azure Depolama** hesabı veya **Veri Gölü Depolama**sı belirtilirsiniz. Bu hesaptan belirli bir depolama kapsayıcısı oluşturduğunuz küme (örneğin, Hadoop Dağıtılmış Dosya Sistemi) için dosya sistemini tutar. Daha fazla bilgi ve rehberlik için bkz:

- [HDInsight ile Azure Depolama'yı kullanma](../hdinsight-hadoop-use-blob-storage.md)
- [Azure HDInsight kümeleriyle Veri Gölü Depolama'yı kullanma](../hdinsight-hadoop-use-data-lake-store.md)

## <a name="use-azure-blob-storage-accounts-with-ml-services-cluster"></a>ML Hizmetleri kümesiyle Azure Blob depolama hesaplarını kullanma

ML Hizmetleri kümenizi oluştururken birden fazla depolama hesabı belirttiyseniz, aşağıdaki yönergeler ml hizmetleri kümesinde veri erişimi ve işlemleri için ikincil bir hesabın nasıl kullanılacağını açıklar. Aşağıdaki depolama hesaplarını ve kapsayıcıyı varsayalım: **depolama1** ve **konteyner1**olarak adlandırılan varsayılan bir kapsayıcı1 ve **kapsayıcı**ile **depolama2** .

> [!WARNING]  
> Performans amacıyla, HDInsight kümesi belirttiğiniz birincil depolama hesabıyla aynı veri merkezinde oluşturulur. HDInsight kümesinden farklı bir konumda bir depolama hesabı nın kullanılması desteklenmez.

### <a name="use-the-default-storage-with-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri ile varsayılan depolama alanını kullanma

1. Bir SSH istemcisi kullanarak, kümenizin kenar düğümüne bağlanın. HDInsight kümeleriyle SSH kullanımı hakkında daha fazla bilgi için [HDInsight ile SSH'yi kullan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.
  
2. Örnek bir dosya, mysamplefile.csv,/share dizinine kopyalayın.

    ```bash
    hadoop fs –mkdir /share
    hadoop fs –copyFromLocal mycsv.scv /share
    ```

3. R Studio veya başka bir R konsoluna geçin ve ad düğümini **varsayılan** olarak ve erişmek istediğiniz dosyanın konumuna ayarlamak için R kodu yazın.  

    ```R
    myNameNode <- "default"
    myPort <- 0

    #Location of the data:  
    bigDataDirRoot <- "/share"  

    #Define Spark compute context:
    mySparkCluster <- RxSpark(nameNode=myNameNode, consoleOutput=TRUE)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define the Hadoop Distributed File System (HDFS) file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")
    ```

Tüm dizin ve dosya başvuruları depolama `wasbs://container1@storage1.blob.core.windows.net`hesabını işaret ediyor. Bu, HDInsight kümesiyle ilişkili **varsayılan depolama hesabıdır.**

### <a name="use-the-additional-storage-with-ml-services-on-hdinsight"></a>HDInsight'ta ML Services ile ek depolama alanını kullanın

Şimdi, **depolama2 container2** /private dizininde bulunan mysamplefile1.csv adlı bir dosya işlemek istediğinizi varsayalım. **storage2**

R kodunuzda, ad düğümü başvurularını **storage2** depolama hesabına yönlendirin.

```R
myNameNode <- "wasbs://container2@storage2.blob.core.windows.net"
myPort <- 0

#Location of the data:
bigDataDirRoot <- "/private"

#Define Spark compute context:
mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

#Set compute context:
rxSetComputeContext(mySparkCluster)

#Define HDFS file system:
hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

#Specify the input file to analyze in HDFS:
inputFile <-file.path(bigDataDirRoot,"mysamplefile1.csv")
```

Tüm dizin ve dosya başvuruları artık depolama `wasbs://container2@storage2.blob.core.windows.net`hesabını işaret ediyor. Bu belirttiğiniz **Ad Düğümü.**

**Depolama2** `/user/RevoShare/<SSH username>` dizinini aşağıdaki gibi yapılandırın:

```bash
hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user
hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user/RevoShare
hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>
```

## <a name="use-azure-data-lake-storage-with-ml-services-cluster"></a>ML Hizmetleri kümesiyle Azure Veri Gölü Depolama'yı kullanma

HDInsight kümenizle Veri Gölü Depolama'yı kullanmak için kümenize kullanmak istediğiniz her Azure Veri Gölü Depolama alanına erişim vermeniz gerekir. Varsayılan depolama alanı olarak veya ek depolama alanı olarak Azure Veri Gölü Depolama hesabı olan bir HDInsight kümesi oluşturmak için Azure portalının nasıl kullanılacağına ilişkin talimatlar için [bkz.](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)

Daha sonra, önceki yordamda açıklandığı gibi ikincil bir Azure depolama hesabı yaptığınız gibi, R komut dosyanızdaki depolama alanını da kullanırsınız.

### <a name="add-cluster-access-to-your-azure-data-lake-storage"></a>Azure Veri Gölü Depolama'nıza küme erişimi ekleme

HDInsight kümenizle ilişkili bir Azure Etkin Dizin (Azure AD) Hizmet Sorumlusu kullanarak Veri Gölü Depolama'ya erişebilirsiniz.

1. HDInsight kümenizi oluştururken, **Veri Kaynağı** sekmesinden **Cluster AAD Identity'i** seçin.

2. Küme **AAD Kimlik** iletişim kutusunda, **AD Hizmet Anası Seç'in**altında, yeni **oluştur'u**seçin.

Hizmet Müdürüne bir ad verdikten ve bunun için bir parola oluşturduktan sonra, Hizmet Sorumlusu'nu Veri Gölü Depolamanızla ilişkilendirmek için **ADLS Erişimini Yönet'i** tıklatın.

Küme oluşturmayı takiben bir veya daha fazla Veri Gölü depolama hesabına küme erişimi eklemek de mümkündür. Veri Gölü Depolaması için Azure portal girişini açın ve **Veri Gezgini > Access > Ekle'ye**gidin.

### <a name="how-to-access-data-lake-storage-gen1-from-ml-services-on-hdinsight"></a>HDInsight'ta ML Services'ten Data Lake Storage Gen1'e nasıl erişilir?

Data Lake Storage Gen1'e erişim izni verdikten sonra, ML Hizmetleri kümesindeki depolama alanını HDInsight'ta ikincil bir Azure depolama hesabı gibi kullanabilirsiniz. Tek fark, önek **wasbs://** aşağıdaki gibi **adl://** değişiklikler olmasıdır:

```R
# Point to the ADL Storage (e.g. ADLtest)
myNameNode <- "adl://rkadl1.azuredatalakestore.net"
myPort <- 0

# Location of the data (assumes a /share directory on the ADL account)
bigDataDirRoot <- "/share"  

# Define Spark compute context
mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

# Set compute context
rxSetComputeContext(mySparkCluster)

# Define HDFS file system
hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# Specify the input file in HDFS to analyze
inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")
```

Aşağıdaki komutlar, Veri Gölü Depolama Gen1 hesabını RevoShare dizini ile yapılandırmak ve önceki örnekten örnek .csv dosyasını eklemek için kullanılır:

```bash
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/mysamplefile.csv adl://rkadl1.azuredatalakestore.net/share

hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share
```

## <a name="use-azure-file-storage-with-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri ile Azure Dosya depolamayı kullanma

Ayrıca, kenar düğümünde kullanmak için [Azure Dosyaları](https://azure.microsoft.com/services/storage/files/)adı verilen kullanışlı bir veri depolama seçeneği de vardır. Linux dosya sistemine bir Azure Depolama dosya paylaşımı monte etmenizi sağlar. Bu seçenek, özellikle HDFS yerine kenar düğümünde yerel dosya sistemini kullanmak mantıklı olduğunda, veri dosyalarını, R komut dosyalarını ve daha sonra ihtiyaç duyulabilen sonuç nesnelerini depolamak için kullanışlı olabilir.

Azure Dosyaları'nın en önemli yararı, dosya paylaşımlarının Windows veya Linux gibi desteklenen işletim sistemine sahip herhangi bir sistem tarafından monte edilebiliyor ve kullanılabilmesi. Örneğin, sizin veya ekibinizdeki bir kişinin sahip olduğu başka bir HDInsight kümesi tarafından, bir Azure VM, hatta şirket içi bir sistem tarafından kullanılabilir. Daha fazla bilgi için bkz.

- [Azure Dosya depolamayı Linux ile kullanma](../../storage/files/storage-how-to-use-files-linux.md)
- [Windows'da Azure Dosyası depolama alanı nasıl kullanılır?](../../storage/files/storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight'ta ML Hizmetleri kümesine Genel Bakış](r-server-overview.md)
- [HDInsight üzerinde ML Services kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
- [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
